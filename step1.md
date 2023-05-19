# インターネットTV
好きな時間に好きな場所で話題の動画を無料で楽しめる「インターネットTVサービス」を新規に作成することになりました。データベース設計をした上で、データ取得する SQL を作成してください。

仕様は次の通りです。サービスのイメージとしては ABEMA を頭に思い浮かべてください。

ドラマ1、ドラマ2、アニメ1、アニメ2、スポーツ、ペットなど、複数のチャンネルがある
各チャンネルの下では時間帯ごとに番組枠が1つ設定されており、番組が放映される
番組はシリーズになっているものと単発ものがある。シリーズになっているものはシーズンが1つものと、シーズン1、シーズン2のように複数シーズンのものがある。各シーズンの下では各エピソードが設定されている
再放送もあるため、ある番組が複数チャンネルの異なる番組枠で放映されることはある
番組の情報として、タイトル、番組詳細、ジャンルが画面上に表示される
各エピソードの情報として、シーズン数、エピソード数、タイトル、エピソード詳細、動画時間、公開日、視聴数が画面上に表示される。単発のエピソードの場合はシーズン数、エピソード数は表示されない
ジャンルとしてアニメ、映画、ドラマ、ニュースなどがある。各番組は1つ以上のジャンルに属する
KPIとして、チャンネルの番組枠のエピソードごとに視聴数を記録する。なお、一つのエピソードは複数の異なるチャンネル及び番組枠で放送されることがあるので、属するチャンネルの番組枠ごとに視聴数がどうだったかも追えるようにする
番組、シーズン、エピソードの関係について、以下のようなイメージです(シリーズになっているものの例)。

番組：鬼滅の刃
シーズン：1
エピソード：1話、2話、...、26話


# ステップ1
テーブル設計をしてください。

テーブルごとにテーブル名、カラム名、データ型、NULL(NULL OK の場合のみ YES と記載)、キー（キーが存在する場合、PRIMARY/INDEX のどちらかを記載）、初期値（ある場合のみ記載）、AUTO INCREMENT（ある場合のみ YES と記載）を記載してください。また、外部キー制約、ユニークキー制約に関しても記載してください。

その際に、少なくとも次のことは満たしてください。

アプリケーションとして成立すること(プログラムを組んだ際に仕様を満たして動作すること)
正規化されていること

# Tables

## Broadcast Schedules

| Field | Type | Null | Key | Default | Extra |
| --- | --- | --- | --- | --- | --- |
| ScheduleID | int | NO | PRI | NULL | auto_increment |
| ChannelID | int | NO | MUL | NULL |  |
| EpisodeID | int | NO | MUL | NULL |  |
| DayOfWeek | char(255) | NO |  | NULL |  |
| StartTime | time | NO |  | NULL |  |
| EndTime | time | NO |  | NULL |  |
| BroadcastDate | date | NO |  | 2023-05-18 |  |

## Channels

| Field | Type | Null | Key | Default | Extra |
| --- | --- | --- | --- | --- | --- |
| ChannelID | int | NO | PRI | NULL | auto_increment |
| ChannelName | char(255) | NO |  | NULL |  |

## Episodes

| Field | Type | Null | Key | Default | Extra |
| --- | --- | --- | --- | --- | --- |
| EpisodeID | int | NO | PRI | NULL | auto_increment |
| SeasonID | int | YES | MUL | NULL |  |
| SeasonNumber | int | YES |  | NULL |  |
| EpisodeNumber | int | YES |  | NULL |  |
| Title | text | NO |  | NULL |  |
| EpisodeDetail | text | NO |  | NULL |  |
| Duration | time | NO |  | NULL |  |
| ReleaseDate | date | NO |  | NULL |  |
| ViewCount | int | NO |  | NULL |  |

## Genres

| Field | Type | Null | Key | Default | Extra |
| --- | --- | --- | --- | --- | --- |
| GenreID | int | NO | PRI | NULL | auto_increment |
| GenreName | char(255) | NO |  | NULL |  |

## Program Genre

| Field | Type | Null | Key | Default | Extra |
| --- | --- | --- | --- | --- | --- |
| ProgramID | int | NO | PRI | NULL |  |
| GenreID | int | NO | PRI | NULL |  |

## Programs

| Field | Type | Null | Key | Default | Extra |
| --- | --- | --- | --- | --- | --- |
| ProgramID | int | NO | PRI | NULL | auto_increment |
| Title | text | NO |  | NULL |  |

## Seasons

| Field | Type | Null | Key | Default | Extra |
| --- | --- | --- | --- | --- | --- |
| SeasonID | int | NO | PRI | NULL | auto_increment |
| ProgramID | int | NO | MUL | NULL |  |
| SeasonName | text | NO |  | NULL |  |

## View Counts

| Field | Type | Null | Key | Default | Extra |
| --- | --- | --- | --- | --- | --- |
| ChannelID | int | NO | PRI | NULL |  |
| ScheduleID | int | NO | PRI | NULL |  |
| EpisodeID | int | NO | PRI | NULL |  |
| ViewCount | int | NO |  | NULL |  |
