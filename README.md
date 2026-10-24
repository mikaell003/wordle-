# Wordle+

A single-file, dependency-free Wordle clone with more modes, a real dictionary, and better stats. No build step, no server, no external requests. Open `wordle-plus.html` in a browser and play.

**Live version:** https://claude.ai/artifact/JNmcQjnQUNFGuNmW5h1w7c

## Features

- **Daily or Unlimited mode.** Daily gives everyone the same word each day (seeded from the date). Unlimited lets you play as many words as you like.
- **4, 5 or 6-letter words.** Stats and saved games are tracked separately per length.
- **Dictionary validation.** Guesses must be real words, otherwise the row shakes with "Not in word list".
- **Hard mode.** Revealed hints (green and yellow) must be reused in later guesses. It can only be switched on at the start of a fresh word.
- **Hint button.** Reveals one unsolved letter position, once per word.
- **Statistics.** Played, win %, current streak, best streak and a guess-distribution chart.
- **Share.** Copies or shares an emoji grid of your result.
- **Accessibility and comfort.** Colour-blind palette, light, dark or auto theme, and on-screen plus physical keyboard support.
- **Progress saved.** Refreshing the page restores your current game.

## Running it

Open `wordle-plus.html` directly in any modern browser, or host it anywhere that serves static files (GitHub Pages, Netlify, Cloudflare Pages, a plain web server). Everything lives in that one file.

## How it works

| Piece | Details |
|---|---|
| Answers | Curated common-word lists in the `W` object (a few hundred words per length), so puzzles stay fair. |
| Valid guesses | Larger dictionaries in the `VS` string map, loaded into `V` sets at startup (about 5.5k four-letter, 14.9k five-letter and 22.3k six-letter words). Any word in `W` is also always accepted. |
| Daily word | A hash of `YYYY-MM-DD` plus the word length indexes into the answer list. The date uses UTC, so the daily word changes at the same moment for everyone. |
| Scoring | Standard two-pass algorithm: greens first, then yellows limited by the remaining count of each letter, so duplicate letters score correctly. |
| Storage | `localStorage` keys: `wp_set` (settings), `wp_stats` (statistics), `wp_g_*` (saved games), `wp_seen` (help shown). Every access is wrapped in `try/catch`, so the game still works if storage is blocked. |

## Customising

- **Change the answers:** edit the word arrays in the `W` object. Words must be lowercase, a-z only, and of the matching length.
- **Change the dictionary:** edit the `VS` strings (space-separated, lowercase). Answers do not need to be duplicated there.
- **Change the number of guesses:** the board is built for 6 rows. Search for `r<6` and `r===5` in the script, and adjust the stats array (`d:[0,0,0,0,0,0]`) to match.
- **Colours and theme:** all colours are CSS variables on `:root` at the top of the file.

## Known limitations

- The 4 and 6-letter dictionaries come from a general English word list, so they accept some obscure words. The answers never come from that list.
- The daily word is derived from the date alone, so anyone who reads the source can work out upcoming answers. Fine for casual play, not for competitive use.
- Statistics are per browser and per device.
- Some fixed English-only text (no localisation).

## Credits and licences

- 5-letter guess list: [tabatkins/wordle-list](https://github.com/tabatkins/wordle-list).
- 4 and 6-letter dictionaries: the [`word-list`](https://www.npmjs.com/package/word-list) npm package.

Check each source's licence before using this commercially or redistributing the word data.
