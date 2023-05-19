# ステップ2
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


## ◆手順のドキュメント

・インターネットTVのデータベースを作成します

mysql> CREATE DATABASE Internet_TV;

・データベースを選択します

mysql> USE Internet_TV;

## ★テーブル構築

以下のように記入してテーブルを構築します

### ・チャンネルテーブル

```mysql

CREATE TABLE Channels (
    ChannelID INT AUTO_INCREMENT,
    ChannelName CHAR(255) NOT NULL,
    PRIMARY KEY (ChannelID)
);
```

### ・ジャンルテーブル

```mysql

CREATE TABLE Genres (
    GenreID INT AUTO_INCREMENT,
    GenreName CHAR(255) NOT NULL,
    PRIMARY KEY (GenreID)
);
```

### ・番組テーブル

```mysql

CREATE TABLE Programs (
    ProgramID INT AUTO_INCREMENT,
    Title TEXT NOT NULL,
    PRIMARY KEY (ProgramID)
);
```

### ・番組＆ジャンルの中間テーブル

```mysql

CREATE TABLE Program_Genre (
    ProgramID INT NOT NULL,
    GenreID INT NOT NULL,
    FOREIGN KEY (ProgramID) REFERENCES Programs(ProgramID),
    FOREIGN KEY (GenreID) REFERENCES Genres(GenreID),
    PRIMARY KEY (ProgramID, GenreID)
);
```

### ・シーズンテーブル

```mysql

CREATE TABLE Seasons (
    SeasonID INT AUTO_INCREMENT,
    ProgramID INT NOT NULL,
    SeasonName TEXT NOT NULL,
    PRIMARY KEY (SeasonID),
    FOREIGN KEY (ProgramID) REFERENCES Programs(ProgramID)
);
```

### ・エピソードテーブル

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
```

### ・番組スケジュールテーブル

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
```

### ・視聴数テーブル

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
```

## データの挿入

### ・チャンネルテーブルにデータを挿入

```
mysql> INSERT INTO Channels (ChannelName) VALUES ('Channel A'), ('Channel B'), ('Channel C');
```

### ・ジャンルテーブルにデータを挿入

```
mysql> INSERT INTO Genres (GenreName) VALUES ('Drama'), ('Comedy'), ('Thriller');
```

### ・番組テーブルにデータを挿入

```
mysql> INSERT INTO Programs (Title) VALUES
    ->     ('Program A'), ('Program B'), ('Program C'),
    ->     ('Program D'), ('Program E'), ('Program F'),
    ->     ('Program G'), ('Program H'), ('Program I'),
    ->     ('Program J');
```

### ・番組＆ジャンルの中間テーブルにデータを挿入

```
mysql> INSERT INTO Program_Genre (ProgramID, GenreID) VALUES
    ->     (1, 1), (1, 2), (2, 3), (3, 1),
    ->     (4, 2), (5, 3), (6, 1), (7, 2),
    ->     (8, 3), (9, 1), (10, 2);
```

### ・シーズンテーブルにデータを挿入

```
mysql> INSERT INTO Seasons (ProgramID, SeasonName) VALUES
    ->     (1, 'Season 1'), (2, 'Season 1'), (3, 'Season 1'),
    ->     (4, 'Season 1'), (5, 'Season 1'), (6, 'Season 1'),
    ->     (7, 'Season 1'), (8, 'Season 1'), (9, 'Season 1'),
    ->     (10, 'Season 1');
```

### ・エピソードテーブルにデータを挿入

```
mysql> INSERT INTO Episodes (SeasonID, SeasonNumber, EpisodeNumber, Title, EpisodeDetail, Duration, ReleaseDate, ViewCount) VALUES
    ->     (1, 1, 1, 'Episode 1', 'Details of episode 1', '00:45:00', '2023-01-01', 5000),
    ->     (1, 1, 2, 'Episode 2', 'Details of episode 2', '00:45:00', '2023-01-08', 5500),
    ->     (2, 1, 1, 'Episode 3', 'Details of episode 3', '00:45:00', '2023-01-15', 6000),
    ->     (2, 1, 2, 'Episode 4', 'Details of episode 4', '00:45:00', '2023-01-22', 6500),
    ->     (3, 1, 1, 'Episode 5', 'Details of episode 5', '00:45:00', '2023-01-29', 7000),
    ->     (3, 1, 2, 'Episode 6', 'Details of episode 6', '00:45:00', '2023-02-05', 7500),
    ->     (4, 1, 1, 'Episode 7', 'Details of episode 7', '00:45:00', '2023-02-12', 8000),
    ->     (4, 1, 2, 'Episode 8', 'Details of episode 8', '00:45:00', '2023-02-19', 8500),
    ->     (5, 1, 1, 'Episode 9', 'Details of episode 9', '00:45:00', '2023-02-26', 9000),
    ->     (5, 1, 2, 'Episode 10', 'Details of episode 10', '00:45:00', '2023-03-05', 9500),
    ->     (1, 1, 3, 'Episode 11', 'Details of episode 11', '00:45:00', '2023-03-12', 10000),
    ->     (1, 1, 4, 'Episode 12', 'Details of episode 12', '00:45:00', '2023-03-19', 10500),
    ->     (2, 1, 3, 'Episode 13', 'Details of episode 13', '00:45:00', '2023-03-26', 11000),
    ->     (2, 1, 4, 'Episode 14', 'Details of episode 14', '00:45:00', '2023-04-02', 11500),
    ->     (3, 1, 3, 'Episode 15', 'Details of episode 15', '00:45:00', '2023-04-09', 12000),
    ->     (3, 1, 4, 'Episode 16', 'Details of episode 16', '00:45:00', '2023-04-16', 12500),
    ->     (4, 1, 3, 'Episode 17', 'Details of episode 17', '00:45:00', '2023-04-23', 13000),
    ->     (4, 1, 4, 'Episode 18', 'Details of episode 18', '00:45:00', '2023-04-30', 13500),
    ->     (5, 1, 3, 'Episode 19', 'Details of episode 19', '00:45:00', '2023-05-07', 14000),
    ->     (5, 1, 4, 'Episode 20', 'Details of episode 20', '00:45:00', '2023-05-14', 14500);
```

### ・番組スケジュールテーブルにデータを挿入

```
mysql> INSERT INTO BroadcastSchedules (ChannelID, EpisodeID, DayOfWeek, StartTime, EndTime) VALUES
    -> (1, 1, 'Monday', '20:00:00', '20:45:00'),
    -> (1, 2, 'Monday', '20:00:00', '20:45:00'),
    -> (2, 3, 'Tuesday', '20:00:00', '20:45:00'),
    -> (2, 4, 'Tuesday', '20:00:00', '20:45:00'),
    -> (3, 5, 'Wednesday', '20:00:00', '20:45:00'),
    -> (3, 6, 'Wednesday', '20:00:00', '20:45:00'),
    -> (1, 7, 'Thursday', '20:00:00', '20:45:00'),
    -> (1, 8, 'Thursday', '20:00:00', '20:45:00'),
    -> (2, 9, 'Friday', '20:00:00', '20:45:00'),
    -> (2, 10, 'Friday', '20:00:00', '20:45:00'),
    -> (1, 11, 'Monday', '00:00:00', '00:45:00'),
    -> (2, 12, 'Monday', '01:00:00', '01:45:00'),
    -> (3, 13, 'Monday', '02:00:00', '02:45:00'),
    -> (1, 14, 'Monday', '03:00:00', '03:45:00'),
    -> (2, 15, 'Monday', '04:00:00', '04:45:00'),
    -> (3, 16, 'Monday', '05:00:00', '05:45:00'),
    -> (1, 17, 'Monday', '06:00:00', '06:45:00'),
    -> (2, 18, 'Monday', '07:00:00', '07:45:00'),
    -> (3, 19, 'Monday', '08:00:00', '08:45:00'),
    -> (1, 20, 'Monday', '09:00:00', '09:45:00');    
```

### ・視聴数テーブルにデータを挿入

```
mysql> INSERT INTO ViewCounts (ChannelID, ScheduleID, EpisodeID, ViewCount) VALUES
    -> (1, 1, 1, 5000),
    -> (1, 2, 2, 5500),
    -> (2, 3, 3, 6000),
    -> (2, 4, 4, 6500),
    -> (3, 5, 5, 7000),
    -> (3, 6, 6, 7500),
    -> (1, 7, 7, 8000),
    -> (1, 8, 8, 8500),
    -> (2, 9, 9, 9000),
    -> (2, 10, 10, 9500),
    -> (1, 11, 11, 10000),
    -> (2, 12, 12, 10500),
    -> (3, 13, 13, 11000),
    -> (1, 14, 14, 11500),
    -> (2, 15, 15, 12000),
    -> (3, 16, 16, 12500),
    -> (1, 17, 17, 13000),
    -> (2, 18, 18, 13500),
    -> (3, 19, 19, 14000),
    -> (1, 20, 20, 14500);
```

