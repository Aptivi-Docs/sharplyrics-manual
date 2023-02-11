---
description: How does it work?
---

# ⚒ How it works

SharpLyrics starts working on your provided lyric file by reading all the lines. Then, it checks to see if the line isn't one of the following:

* An empty line
* A non-lyric line
* A lyric line without a lyric line

If the line is really a lyric line, then it starts processing it by taking the time span from the line and parsing it to a `TimeSpan` and taking the lyrics from the line. Below shows the clarification of this process:

```
[01:13.40]Don't believe me, just watch!
 ~~~~~~~~ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  |        |
  |        \- Lyric line
  \---------- Time of the lyric
```

In case the lyric line contains time spans of when the words are said, defined like below:

```
[02:55.75]<02:55.75> Uptown <02:56.75> funk <02:56.95> you <02:57.05> up!
 ~~~~~~~~ ~~~~~~~~~~ ~~~~~~
  |        |          |
  |        |          \- Word to be said
  |        \------------ Time of when the word is said
  \--------------------- Time of when the line is said
```

SharpLyrics will attempt to split these words to their time spans, or `00:00` if these time spans can't be found in the line.

Once this is done, SharpLyrics will install all the lyric lines (`LyricLine` class) to the list of lines in a new `Lyric` class instance, therefore returning it for usage in your applications.

## An example

If you want a simple console app that lets you print the lyric lines as they play to simulate the lyric player feature usually found in your music player, the most minimal example of such an application is this:

```csharp
string path = "path/to/music.lrc";
var lyric = LyricReader.GetLyrics(path);
var lyricLines = lyric.Lines;
var shownLines = new List<LyricLine>();
var sw = new Stopwatch();

sw.Start();
foreach (var ts in lyricLines)
{
    while (sw.Elapsed < ts.LineSpan)
        Thread.Sleep(1);

    if (sw.Elapsed > ts.LineSpan)
    {
        Console.WriteLine(ts.Line);
        shownLines.Add(ts);
        if (shownLines.Count == lyricLines.Count)
            return;
    }
}
```

To view the source code of the full demo, check [the source code out](https://github.com/Aptivi/SharpLyrics/blob/main/SharpLyrics.Demo/SyncWithLyrics.cs).
