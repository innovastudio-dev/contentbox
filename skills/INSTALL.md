# Install the contentbox-design skill

These steps are for an AI coding assistant (Claude Code, Cursor and the like).
Follow them in order.

1. Download the skill:
   https://github.com/innovastudio-dev/contentbox/raw/HEAD/skills/contentbox-design.zip
2. Unzip it. It holds one folder, `contentbox-design`, with `SKILL.md` inside.
3. Put that folder in your skills folder. If a `contentbox-design` folder is already
   there, replace it: this is an update.
   - Claude Code: `~/.claude/skills/contentbox-design` (all projects). Use the project's
     `.claude/skills/contentbox-design` instead only if the user asks for this project only.
   - Another assistant that reads skill folders (a folder with a `SKILL.md`): its own
     skills folder.
   - An assistant without skill folders: stop, and tell the user to follow the steps at
     https://github.com/innovastudio-dev/contentbox/blob/HEAD/skills/README.md instead.

   On macOS or Linux, for Claude Code:

   ```bash
   mkdir -p ~/.claude/skills && cd ~/.claude/skills \
     && curl -fsSL -o contentbox-design.zip https://github.com/innovastudio-dev/contentbox/raw/HEAD/skills/contentbox-design.zip \
     && rm -rf contentbox-design && unzip -q contentbox-design.zip && rm contentbox-design.zip
   ```

4. Check that `contentbox-design/SKILL.md` exists in the skills folder.
5. Tell the user it is installed, and how to use it: ask for a ContentBox page or
   section, for example *"Create a ContentBox landing page for a coffee roastery"*.
