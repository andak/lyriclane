# Lyric Lane

A single static page that renders a marked-up lyric with one lane per singer,
so everyone can see who sings what.

## Run locally

```sh
cd ~/repo/lyrics
python -m http.server
```

Open <http://localhost:8000/>. The page cannot read the song files when opened
directly from disk (browsers block `fetch` on `file://`), so it must be served.
Any static host works too, for example GitHub Pages.

## Links

- Overview, all singers: `index.html?song=songs/chris-holsten-sla-hjerte-sla.txt`
- One singer: `index.html?song=songs/chris-holsten-sla-hjerte-sla.txt#Amanda+Ida`
- All sheets, for printing: `index.html?song=songs/chris-holsten-sla-hjerte-sla.txt#*`

Print with Ctrl+P. From the overview ("All") this prints the whole pack: the
overview followed by one sheet per singer. From a singer's tab it prints that
singer's sheet only. The all-sheets link shows the whole pack on screen.

## Markup

```
title: Trio tune
singers: Anna, Live, Anders

              // verse
[Live]        I know about a song
              Where we all sing along
[Anna]        Sometimes it's me
[Live/Anna]   Sometimes it's two
[*]           Sometimes it's everybody
[Anders]      Sometimes it's only you

              // interlude (guitar)
[]            ♪

              // chorus
[*]           And hey ho, there is no cure
              for bad lyrics, that's for sure
              let me just show what this thing can do
              so we can stop torturing us through
[Anna/Anders] these bad lyrics - boo
```

- `title:` and `singers:` header lines come first. Singers are comma separated
  and their order is the lane order. A singer name is any text without a comma
  or slash.
- `singers:` may be left out when everybody sings everything. The song then
  renders without lanes or singer tabs, and lines need no tags.
- `[name/name]` at the start of a line sets who sings, from that line onwards.
- `[*]` means every declared singer.
- `[]` means nobody, for example an instrumental bridge: `[] ♪`
- A line without a tag inherits the previous tag, also across blank lines and
  notes.
- `//` starts a note. It is shown in every view, for section names or anything
  the singers need to know.
- Blank lines separate stanzas.
- Whitespace before the lyric text is ignored, so align tags however you like.

Unknown singer names and lyric lines before the first tag are reported as
errors at the top of the page. Cyrillic letters mixed into Latin text (a
common artifact from lyric websites) are reported as warnings.

Add new songs to `songs/` and list them in `songs.txt`, one path per line.
