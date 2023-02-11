---
description: How do you use it?
---

# 🖥 How to use

Using this library is very simple! Just use the `SharpLyrics` namespace in any piece of code you want to use the library, as in: `using SharpLyrics;`

Just use the `LyricReader` class that contains:

* `GetLyrics(string)`

The string in this function must be a complete path to your `.lrc` or `.txt` file containing lyric information about your music. It can be of a standard form (only time in lines) and an extended form (time in lines and words).

Once called, it returns a `Lyric` that contains information about your lyric, including a list of lyric lines defined with the `LyricLine` class. It contains:

* `string Line`: A lyric line
* `TimeSpan LineSpan`: Time of when a lyric line gets played
* `List<LyricLineWord> LineWords`: Group of words from the lyric line

`LyricLineWord` also contains these variables:

* `string Word`: A lyric word
* `TimeSpan WordSpan`: Time of when a word gets said
