---
name: makefun_recording_seal
description: Record and seal make.fun build sessions — one recording per entry; seal by Koding as the last step.
---

# Make.fun Recording and Seal

## Start the recording before any build work

- Start the recording before the builder writes any code or runs any build command.

- Check that the recorder is running and that the builder session is inside the recorded set before the builder starts work.

- Stop and restart the recording at once if the recorder dies, and treat any work done while it was dead as unwitnessed.

- Do not submit a commit whose build time was unwitnessed, and rebuild it under a live recording instead.

- Run the bundle check the makefun skill describes after the first build steps, and fix every missing usage row before the build continues.

## Match the kit level and keep one session document per entry

- Check the kit level on your machine against the level the site reports before you start a recording.

- Upgrade the kit to the site level when your local kit is older, and do not start a recording on the older kit.

- Use one recording for each entry, and never bundle two recordings that share a session document.

- List the session documents in every recording before you submit, and stop when the same document appears in more than one recording.

- Discard a session document that a dead recorder left with no code written, and start a fresh recording instead of letting the next recording reuse it.

- Rebuild the affected work under a single live recording on the matched kit when a submit is refused for a duplicate document, and submit only that single recording.

## Keep the lead silent while recording

- Make zero tool calls from the lead session between record start and the sealed result, and let the builder do all work in that window.

- Keep the coordinator out of the sealing set, and never seal from the lead or coordinator session.

- Put the seal instruction in the builder brief before the recording starts, and name the builder as the only sealer.

- Wait for the builder result before you verify the seal, and restart paused work only after the seal is confirmed.

## Let the builder seal as its last step

- Have the builder seal as its final step, after commit and push and after it writes its usage rows.

- Check that every session in the sealing set carries token usage from the model provider before you seal, and never estimate usage by hand.

- Confirm the recording shows sealed before you report the submission as ready, and report held instead of sealed when the seal does not confirm.

- Treat a plain retry of a failed seal as no fix, and find the missing usage row or session before you seal again.

## Check slots and orphans before a new recording

- Check how many recording slots are in use before you start a new recording, and free a slot before you start when the count is full.

- Treat sealing an orphan with an empty markdown file as a workaround for freeing a slot, and use it only when the platform offers that path.

- Never start a new recording while an earlier one is still held, and resolve the held recording first.

## Available connectors

- Use muse.connector_search to find other connectors and muse.skill_search to find related skills.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up with the Custom Connectors and MCPs skill, or build a custom connector.

## Alternatives for better results

- Load the makefun skill before you start a bounty, and follow its bundle and check steps before you submit.

- Build in a harness whose log the kit bundle reads, so usage rows exist without hand editing.

- Keep the builder session short and seal promptly, so a recorder death loses minutes instead of most of a build.