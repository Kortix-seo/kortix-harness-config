# harnesses/

Files that only one harness implementation reads, in an open-source Kortix project. Kortix is open source; these files are read by the loop that runs your agents.

The project root is harness-neutral: `agents/`, `skills/`, `memory/`, `tools/` and `kortix.yaml` are read by whichever harness a session runs. Anything an implementation needs that no other implementation would understand — a config file, a hook, a prompt preamble in that loop's dialect — belongs in its own folder here, named after the implementation.

Two rules:

- **One box, one harness, for the life of the box.** A session resolves its harness at boot; there is no per-request switching. An unknown implementation fails the boot rather than falling back to a default.
- **Nothing in here may hold a grant.** Permissions live in `kortix.yaml`, so a harness swap cannot change what an agent may touch. If a file in this folder needs a credential, the design is wrong: move the decision to the manifest.

This folder is empty on purpose. Add a subfolder only when a specific implementation needs a file that the neutral layout cannot express.
