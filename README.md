# browserquest-server

A Java server implementation for Mozilla's [BrowserQuest](https://github.com/mozilla/BrowserQuest) — the 2012 HTML5 MMORPG tech demo. Reimplements the original Node.js server in Java with an entity-component-system core, behaviour trees for mob AI, and a Netty-based network layer.

## Why it exists

BrowserQuest shipped with a minimal server that was fine for a tech demo and thin for anything else. This is what the server might look like if you cared about data-oriented design, extensibility, and long-running stability — ECS instead of class hierarchies, behaviour trees instead of switch-on-state, OSGi modules instead of monolithic deploy.

Protocol-compatible with the stock BrowserQuest web client.

## Architecture

Multi-module Maven project:

- **`boot`** — entry point, configuration, lifecycle.
- **`commons`** — shared DTOs, protocol definitions, utility classes.
- **`ecs`** — entity-component-system runtime (wraps [Ashley](https://github.com/libgdx/ashley)).
- **`server`** — game logic: entities, brains (gdx-ai behaviour trees), combat, inventory, items, world, packets.

Networking is [Netty](https://netty.io) with the BrowserQuest message framing. Serialization is Jackson over the wire. Modules are loaded and hot-swapped via Apache Felix OSGi.

Mob AI is composed of behaviour trees (gdx-ai) rather than finite state machines; mobs can compose new behaviours without subclassing.

## Build

```bash
mvn clean install
java -jar boot/target/browserquest-server.jar
```

Point a stock BrowserQuest client at `ws://localhost:8000/` and it should connect.

## What's implemented

- Player movement, chat, inventory, combat.
- Mob spawning, pathfinding, aggro, behaviour-tree AI.
- Item pickup, drop tables, equipment.
- Multiple maps, zone transitions.

## What's not

- No persistence. Player state is in-memory; restart wipes the world.
- No matchmaking, no instances, no sharding.
- No anti-cheat. The original BrowserQuest wasn't competitive and neither is this.
- No admin tools beyond a minimal console.

## Credit

- [BrowserQuest](https://github.com/mozilla/BrowserQuest) — Mozilla, 2012. MPL 2.0. This server is an independent reimplementation; no Mozilla code is included.
- [Ashley ECS](https://github.com/libgdx/ashley), [gdx-ai](https://github.com/libgdx/gdx-ai), [Netty](https://netty.io), [Apache Felix](https://felix.apache.org/), [Jackson](https://github.com/FasterXML/jackson).

## License

MIT.
