# Reproducing failed schema apply

This demo reproduces this error:

```
error: alter table "articles" alter column "id" drop not null - column "id" is in a primary key
    at Parser.parseErrorMessage (/.../node_modules/pg-protocol/dist/parser.js:287:98)
    at Parser.handlePacket (/.../node_modules/pg-protocol/dist/parser.js:126:29)
    at Parser.parse (/.../node_modules/pg-protocol/dist/parser.js:39:38)
    at Socket.<anonymous> (/.../node_modules/pg-protocol/dist/index.js:11:42)
    at Socket.emit (node:events:513:28)
    at addChunk (node:internal/streams/readable:324:12)
    at readableAddChunk (node:internal/streams/readable:297:9)
    at Readable.push (node:internal/streams/readable:234:10)
    at TCP.onStreamRead (node:internal/stream_base_commons:190:23)
```

Feel free to swap `pnpm` with `yarn` or `npm`.

```sh
# Install dependencies
pnpm install

# Setup .env (may need to tweak some settings)
cp .env.example .env

# Bootstrap project
pnpm run bootstrap

# Apply first snapshot
pnpm run schema:apply

# Apply second snapshot and observe error
pnpm run schema:apply_v2
```

The only difference between the two snapshots are the `sort` fields. It appears Directus attempts to alter the `id` column even though the definition has not changed. Since `sort` is only a visual difference in the Directus UI, one would expect the `articles` table would not be altered.
