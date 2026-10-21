# MyPreflight knowledge base

[![product](.github/image/banner.png)](https://mypreflight.io)

What MyPreflight is, how it's built and how we decide things. Written for contributors and the agents they work with.

Last reviewed: 2026-10-20

MyPreflight turns a SimBrief flight plan into a full airline-style briefing (passengers with names and special needs, cargo like salmon from Keflavik, a loadsheet, the real seat map, plain-language weather, NOTAMs, gate and stand) and tracks the flight from off-block to on-block through a tiny Windows connector. It runs as a second screen on a tablet or phone while you fly Microsoft Flight Simulator.

- App: app.mypreflight.io
- Website and docs: mypreflight.io
- All MyPreflight code is open source. See `repos.md`.

## Using this with Claude

Point your Claude at `CLAUDE.md` in this repo by its raw GitHub URL:

```text
https://raw.githubusercontent.com/mypreflight/product/main/CLAUDE.md
```

Code repos point here from a small `CLAUDE.md` of their own, built from `technology/templates/code-repo-CLAUDE.md`.

## Contributing

- Found something wrong or out of date? Open an issue or, better, a PR with the fix.
- Have an idea? Check `ideas-parking-lot.md`, then open an issue.
- Product decisions are made by the product owner. A PR can propose a decision; merging it is his call.
- CI runs markdownlint on every PR, with the rules in `.markdownlint-cli2.jsonc`.

## Licence

- The text of this knowledge base is licensed under [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0). See `LICENSE`.
- The MyPreflight name, logo and everything in `product/brand/assets/` are not covered by that licence. All rights reserved.

For flight simulation use only.
