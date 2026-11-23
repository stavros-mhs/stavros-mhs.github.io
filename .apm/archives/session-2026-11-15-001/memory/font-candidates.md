# Font candidates

Stage 2, Task 2.1, 2026-11-13.

**Chosen:** IBM Plex Sans, the one typeface for the whole site. The User first picked IBM Plex Mono for its sharp, typewriter-like look, then compared it with Plex Sans and Literata, and chose Plex Sans as cleaner and more readable.

**Keep this file to refresh the font later.** To try another candidate, change the Google Fonts `<link>` in `index.html` to the query shown below, then change `font-family` on `body` in `style.css`.

The trial page is in Git history, so it can be restored with `git show 715aa05:trial-fonts.html`. Commit `9640671` holds the first five candidates, and `715aa05` holds the 12 below.

## Monospace, close to JetBrains Mono

| Font | Note | Google Fonts query |
|------|------|--------------------|
| IBM Plex Mono | **Chosen.** Sharp, typewriter-like. | `family=IBM+Plex+Mono` |
| JetBrains Mono | First reference pick. Not chosen. | `family=JetBrains+Mono` |
| Fira Code | Close to JetBrains Mono in feel. | `family=Fira+Code` |
| DM Mono | Softer and more personal. | `family=DM+Mono` |
| Geist Mono | Clean and modern. | `family=Geist+Mono` |
| Red Hat Mono | Technical, slightly rounder. | `family=Red+Hat+Mono` |

## Serif, close to Newsreader

| Font | Note | Google Fonts query |
|------|------|--------------------|
| Newsreader | First reference pick. Not chosen. | `family=Newsreader` |
| Literata | Built for long reading, warm. | `family=Literata` |
| Source Serif 4 | Sturdy and neutral. | `family=Source+Serif+4` |
| Spectral | Sharper serif. | `family=Spectral` |
| Crimson Pro | Classic book serif, lighter weight. | `family=Crimson+Pro` |
| EB Garamond | Traditional Garamond. | `family=EB+Garamond` |

## Earlier round, rejected

- IBM Plex Sans: the original body font. Chosen again after comparison with Plex Mono and Literata.
- Inter
- Source Sans 3

## Notes for later

- A monospace body text is wider than a sans, so line length and readability at the 800px content width need a check in Task 3.1.
