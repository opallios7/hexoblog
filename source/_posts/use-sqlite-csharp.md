---
title: C#에서 SQLite3 사용하기
date: 2017-10-18 15:35:07
thumbnail: /assets/Images/dev/dotnet/sqlite_banner.gif
tags:
  - C#
  - .NET
  - SQLite
  - 씨샵
  - 닷넷
  - 로컬DB
categories:
  - 개발(dev)
  - c#(c#)
---

SQLite는 라는 것을 오래전부터 들어왔는데 그동안 Access라는 MS의 강력하고 편리한 로컬DB를 활용하다보니 크게 사용할 일이 없었다. 하지만 Access는 유료이기에 자유롭게 사용하는데 제약이 있고 또 다양한 플렛폼에서 사용할 수 없는 단점이 있다. 그래서 이번 기회에 SQLite라는 것을 이용해서 프로그램을 만들어 보자고자 맘 먹고 공부를 시작한다.

## 개요
**"SQLite는 자체 내장, 높은 안정성, 모든 기능을 갖춘 퍼블릭 도메인의 SQL 데이터베이스 엔진입니다. SQLite는 세계에서 가장 많이 사용되는 데이터베이스 엔진입니다."** 라고 SQLite 홈페이지에 나와 있다. 좀더 자세히 알고 싶다면 다음의 URL [SQLite 홈페이지](https://www.sqlite.org)를 참고하길 바란다.

## SQLite GUI 관리 툴
MSSQL에 Microsoft SQL Server Management Studio가 있고 ORACLE에 SQL Developer가 있듯 SQLite도 편리하게 관리할 수 있는 GUI 툴이 존재한다. 구글을 해보니 다양한 프로그램들이 있었다. 무료도 있고 유료도 많이 존재했다. 가장 대표적인 툴이 [DB Browser for SQLite](http://sqlitebrowser.org)라는 프로그램이었다. 간단하게 사용해본 결과 만족스러웠지만 한가지 앞으로 이야기할 것이지만 .NET용 라이브러리로 암호화 된 DB파일이 열리지 않았다. DB Browser는 SQLCipher라는 별도의 라이브러리를 사용하여 만들어져 있어 서로 호환이 되지 않아 다른 것을 찾았다. 한참을 찾다 작년까지 업데이트 되던 [SQLite Studio](https://sqlitestudio.pl/index.rvt)라는 프로그램을 찾았다. 잠깐 사용해보니 이 프로그램은 SQLite에서 사용하는 암호화 라이브러리를 전부 지원하는 것 같다. 

## Visual Studio SQLite 라이브러리 설치
Visual Studio에서 SQLite 라이브러이의 설치는 생각보다 간단하다. 내 기억속에는 Visual Studio 2012(정확하지는 않음)부터 Nuget이 내장되어있어 Nuget을 통해 해당 라이브러리를 찾아 설치를 누르면 간단하게 설치된다. (이것이 싫으면 직접 찾아서 설치하여 해당 라이브러리를 참조를 통해 가져오면 되는데 여기선 생략한다.)

다음의 메뉴에서 솔루션용 Nuget 패키지 관리를 실행한다.
<p><img src="/assets/Images/dev/dotnet/run_nuget_module.png" alt="Visual Studio에서 Nuget 실행" title="Nuget 실행" width=500 height=400/></p>

찾아보기로 가서 **"SQLite"**를 입력하고 검색하여 설치를 누르면 Visual Studio에서는 다음과 같이 종속라이브러리를 찾아 설치한다.
<p><img src="/assets/Images/dev/dotnet/install_sqlite_in_visualstudio.png" alt="Visual Studio에서 Sqlite 라이브러리 설치" title="Sqlite 라이브러리 설치 " width=500 height=400/></p>

설치가 완료가 되면 해당 프로젝트에서 사용할 준비가 완료되었다고 생각하면 된다.

## DB 연결 및 종료
SQLite를 연결하는 방법은 ConnectionString만 정의하면 나머진 .NET에서 제공하는 MySQL이나 MSSQL, ORACLE등의 사용과 라이브러리와 똑같이 사용할 수 있다. 약간의 차이는 있겠지만 기본적인 작업을 한다면 똑같다고 생각해도 큰 무리는 없어보인다.

다음의 코드는 DB를 연결하는 방법이다.

```java
SQLiteConnection conn = new SQLiteConnection("Data Source=sqlite 파일 경로;Version=3;");
conn.open();  // DB연결

// 이곳에 필요한 쿼리를 호출하여 사용

conn.Close(); // DB종료  
```

## 데이터 조회(Select)
데이터를 조회하는 방법의 코드은 다음과 같다.

```java
SQLiteCommand cmd = new SQLiteCommand("SELECT name, value FROM sample_table", conn);
  
SQLiteDataReader dr = cmd.ExecuteReader();
if (dr.HasRows)
{
    while(dr.read())
    {
      Console.WriteLine(dr["name"].ToString());
    }
}
dr.Close();
```

## 데이터 입력/수정/삭제 (Insert/Update/Delete)
다음은 데이터를 입력하는 방법의 코드이다.(수정/삭제는 쿼리만 해당 트랜잭션에 맞게 변경하여 사용하면 된다.)

```java
SQLiteCommand cmd = new SQLiteCommand("INSERT INTO sample_table ( name, value ) VALUES ( @name, @value)", conn);
cmd.Parameters.Add("@name", DbType.String);
cmd.Parameters.Add("@value", DbType.String);
cmd.Prepare();

cmd.Parameters[0].Value = tbName.Text;
cmd.Parameters[1].Value = tbValue.Text;
cmd.ExecuteNonQuery();
```

## DB파일 암호화
파일암호화는 생각보다 아주 쉬웠다. 다음의 딱 두가지 메소드만 알면 암호화를 하고 암호인증을 하여 접근이 가능하다. 
- SetPassword(databasePassword) : string 또는 byte[] 타입의 파라메터 사용
- ChangePassword(newPassword) : string 또는 byte[] 타입의 파라메터 사용

다음은 SQLite파일이 존재한다는 가정하에 해당 파일을 암호화 하는 코드이다.

```java
SQLiteConnection conn = new SQLiteConnection("Data Source=sqlite 파일 경로;Version=3;");
conn.Open();
conn.ChangePassword("패스워드");
.
.
.
conn.Close();
```

다음은 암호화가 걸려있는 SQLite파일을 인증하는 코드이다.
```java
SQLiteConnection conn = new SQLiteConnection("Data Source=sqlite 파일 경로;Version=3;");
conn.SetPassword("패스워드");
conn.Open();
.
.
.
conn.Close();
```

암호가 틀렸을 경우 디버그 모드로 보면 다음과 같음 메시지가 나타난다.
<p><img src="/assets/Images/dev/dotnet/show_errormsg_sqlite_wrong_password.png" alt="SQLite 패스워드 오류 메시지" title="SQLite 패스워드 오류 메시지" width=300 height=200/></p>




