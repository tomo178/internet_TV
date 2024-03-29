# ◆ステップ3

以下のデータを抽出するクエリを書いてください。

### ①よく見られているエピソードを知りたいです。エピソード視聴数トップ3のエピソードタイトルと視聴数を取得してください

```
mysql> SELECT Title, ViewCount
    -> FROM Episodes
    -> ORDER BY ViewCount DESC
    -> LIMIT 3;
```

### ②よく見られているエピソードの番組情報やシーズン情報も合わせて知りたいです。エピソード視聴数トップ3の番組タイトル、シーズン数、エピソード数、エピソードタイトル、視聴数を取得してください

```
SELECT Programs.Title, Episodes.SeasonNumber, Episodes.EpisodeNumber, Episodes.Title, Episodes.ViewCount
FROM Programs
INNER JOIN Seasons ON Programs.ProgramID = Seasons.ProgramID
INNER JOIN Episodes ON Seasons.SeasonID = Episodes.SeasonID;
ORDER BY ViewCount DESC
LIMIT 3;
```

### ③本日の番組表を表示するために、本日、どのチャンネルの、何時から、何の番組が放送されるのかを知りたいです。本日放送される全ての番組に対して、チャンネル名、放送開始時刻(日付+時間)、放送終了時刻、シーズン数、エピソード数、エピソードタイトル、エピソード詳細を取得してください。なお、番組の開始時刻が本日のものを本日方法される番組とみなすものとします

```
SELECT Channels.ChannelName, BroadcastSchedules.DayOfWeek, BroadcastSchedules.StartTime BroadcastSchedules.EndTime, Episodes.SeasonNumber, Episodes.EpisodeNumber, Episodes.Title, Episodes.EpisodeDetail
FROM BroadcastSchedules 
INNER JOIN Channels ON Channels.ChannelID = BroadcastSchedules.ChannelID 
INNER JOIN Episodes ON Episodes.EpisodeID= BroadcastSchedules.EpisodeID;
WHEREBroadcastSchedules.BroadcastDate='2023-05-22';
```

### ④ドラマというチャンネルがあったとして、ドラマのチャンネルの番組表を表示するために、本日から一週間分、何日の何時から何の番組が放送されるのかを知りたいです。ドラマのチャンネルに対して、放送開始時刻、放送終了時刻、シーズン数、エピソード数、エピソードタイトル、エピソード詳細を本日から一週間分取得してください

```
SELECT
    BroadcastSchedules.BroadcastDate,
    BroadcastSchedules.StartTime,
    BroadcastSchedules.EndTime,
    Episodes.SeasonNumber,
    Episodes.EpisodeNumber,
    Episodes.Title,
    Episodes.EpisodeDetail
FROM
    BroadcastSchedules
INNER JOIN
    Episodes ON Episodes.EpisodeID = BroadcastSchedules.EpisodeID
INNER JOIN
    Channels ON Channels.ChannelID = BroadcastSchedules.ChannelID
WHERE
    BroadcastSchedules.BroadcastDate BETWEEN '2023-05-22' AND '2023-05-28'
AND
    Channels.ChannelName = 'Channel A';

```

### ⑤(advanced) 直近一週間で最も見られた番組が知りたいです。直近一週間に放送された番組の中で、エピソード視聴数合計トップ2の番組に対して、番組タイトル、視聴数を取得してください
未着手


### ⑥(advanced) ジャンルごとの番組の視聴数ランキングを知りたいです。番組の視聴数ランキングはエピソードの平均視聴数ランキングとします。ジャンルごとに視聴数トップの番組に対して、ジャンル名、番組タイトル、エピソード平均視聴数を取得してください。
未着手
