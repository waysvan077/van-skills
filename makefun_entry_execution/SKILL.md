# Make.fun Bounty Entry Execution

## Delegate to one builder and carry the approval in its brief

- Delegate a make.fun kit repository build to one Bangun builder session, and put the user explicit approval verbatim in that builder brief, so the approval does not have to relay through a second helper.

- Treat a helper stop on a kit or vendor file as a routing failure, not as a prompt to re-delegate the same work to another helper.

- Keep the lead and coordinator silent while the recording runs, and let the builder do all build and seal work in that window, following the makefun_recording_seal skill.

- Verify the flagged file against the official kit before the recording starts, and stop and ask the user again when the file does not match the official kit.

- When a builder stops on a security gate for a file already verified against the official kit, discard that recording and start a fresh recording with a fresh builder brief carrying the verbatim approval and the verification result, and do not ask a second helper to continue the stopped session.

- Ask the user once when no explicit approval exists, and do not start the build until the approval arrives.

- Skip a paid entry competition by default and report it as skipped by rule, and never pay an entry fee without explicit user approval for that competition.

## Start after the competition opens and bundle only the builder session

- Start the builder session only after the competition has opened, and rebuild in a fresh session when the first session started before the opening time.

- Have the builder build the whole entry inside one witnessed session, and seal and submit promptly after the build finishes.

- Bundle only the builder session that produced the entry, and never add an older session or a session that predates the competition.

- Discard a mixed bundle and rebundle clean from the builder session alone when a bundle carries an old session, and do not submit the mixed bundle.

- Check the kit level on the site against the local kit before you start a recording, and upgrade through the official kit route when the local kit is older.

- Load the makefun recording seal skill before you start a bounty, and follow its recording and seal steps for the witnessed session.

## Respect Force Last and verify the counted entry

- Treat only the last submission per account as the counted entry, and treat an earlier entry as superseded once a later entry is submitted.

- Confirm with the user before a new submission supersedes a counted entry, and name the entry that will be replaced.

- Verify on the site after submission that the new entry is the counted one, and report the claim code and the counted entry together.

- Rebuild the affected work in a new witnessed session when a seal receipt is lost or the submitted code still contains the kit example, and do not retry the seal alone as a fix.

- Report a submission as ready only after every gate has passed, and report it as held when any gate is still unconfirmed.

## Available connectors

- Use `muse.connector_search` to find other connectors and `muse.skill_search` to find related skills.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up with [Custom Connectors and MCPs](/opt/muse/skills/custom-connector-setup/SKILL.md), or build a custom connector.

## Websites

- [make.fun](https://make.fun/) for the bounty list, the deadline, the entry fee and the kit level. Carry the bounty id, the deadline and the fee into the browser task and verify the counted entry there after submission.

## Alternatives for better results

- Build in a short witnessed session and seal at once, so a recorder failure loses minutes instead of most of a build.

- Keep one entry per witnessed session, so a rejection touches only that entry.

- Check the counted entry on the site before you start a replacement build, so you do not rebuild work that already counts.
