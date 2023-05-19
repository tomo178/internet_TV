ステップ2
実際にテーブルを構築し、データを入れましょう。その手順をドキュメントとしてまとめてください（アウトプットは手順のドキュメントです）。

具体的には、以下のことを行う手順のドキュメントを作成してください。

データベースを構築します
ステップ1で設計したテーブルを構築します
サンプルデータを入れます。サンプルデータはご自身で作成ください（ChatGPTを利用すると比較的簡単に生成できます）
手順のドキュメントは、他の人が見た時にその手順通りに実施すればテーブル作成及びサンプルデータ格納が行えるように記載してください。

なお、ステップ2は以下のことを狙っています。

データを実際に入れることでステップ3でデータ抽出クエリを試せるようにすること
手順をドキュメントにまとめることで、自身がやり直したい時にすぐやり直せること
手順を人が同じように行えるようにまとめることで、ドキュメントコミュニケーション力を上げること


◆手順のドキュメント

・インターネットTVのデータベースを作成
mysql> CREATE DATABASE Internet_TV;

・データベースを選択
mysql> USE Internet_TV;

★テーブル構築

・チャンネルテーブル

```mysql

CREATE TABLE Channels (
    ChannelID INT AUTO_INCREMENT,
    ChannelName CHAR(255) NOT NULL,
    PRIMARY KEY (ChannelID)
);

・ジャンルテーブル

```mysql

CREATE TABLE Genres (
    GenreID INT AUTO_INCREMENT,
    GenreName CHAR(255) NOT NULL,
    PRIMARY KEY (GenreID)
);

・番組テーブル

```mysql

CREATE TABLE Programs (
    ProgramID INT AUTO_INCREMENT,
    Title TEXT NOT NULL,
    PRIMARY KEY (ProgramID)
);

・番組＆ジャンルの中間テーブル

```mysql

CREATE TABLE Program_Genre (
    ProgramID INT NOT NULL,
    GenreID INT NOT NULL,
    FOREIGN KEY (ProgramID) REFERENCES Programs(ProgramID),
    FOREIGN KEY (GenreID) REFERENCES Genres(GenreID),
    PRIMARY KEY (ProgramID, GenreID)
);

・シーズンテーブル

```mysql

CREATE TABLE Seasons (
    SeasonID INT AUTO_INCREMENT,
    ProgramID INT NOT NULL,
    SeasonName TEXT NOT NULL,
    PRIMARY KEY (SeasonID),
    FOREIGN KEY (ProgramID) REFERENCES Programs(ProgramID)
);

・エピソードテーブル

```mysql

CREATE TABLE Episodes (
    EpisodeID INT AUTO_INCREMENT,
    SeasonID INT,
    SeasonNumber INT,
    EpisodeNumber INT,
    Title TEXT NOT NULL,
    EpisodeDetail TEXT NOT NULL,
    Duration TIME NOT NULL,
    ReleaseDate DATE NOT NULL,
    ViewCount INT NOT NULL,
    PRIMARY KEY (EpisodeID),
    FOREIGN KEY (SeasonID) REFERENCES Seasons(SeasonID)
);

・番組スケジュールテーブル

```mysql

CREATE TABLE BroadcastSchedules (
    ScheduleID INT AUTO_INCREMENT,
    ChannelID INT NOT NULL,
    EpisodeID INT NOT NULL,
    DayOfWeek CHAR(255) NOT NULL,
    StartTime TIME NOT NULL,
    EndTime TIME NOT NULL,
    BroadcastDate DATE NOT NULL DEFAULT '2023-05-18',
    PRIMARY KEY (ScheduleID),
    FOREIGN KEY (ChannelID) REFERENCES Channels(ChannelID),
    FOREIGN KEY (EpisodeID) REFERENCES Episodes(EpisodeID)
);

・視聴数テーブル

```mysql

CREATE TABLE ViewCounts (
    ChannelID INT NOT NULL,
    ScheduleID INT NOT NULL,
    EpisodeID INT NOT NULL,
    ViewCount INT NOT NULL,
    FOREIGN KEY (ChannelID) REFERENCES Channels(ChannelID),
    FOREIGN KEY (ScheduleID) REFERENCES BroadcastSchedules(ScheduleID),
    FOREIGN KEY (EpisodeID) REFERENCES Episodes(EpisodeID),
    PRIMARY KEY (ChannelID, ScheduleID, EpisodeID)
);
