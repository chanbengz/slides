# slides

A template repo for slidev slides with automatic GitHub Pages deployment.

## Usage

1. Duplicate the `template` folder in the `slides` folder and rename it to your presentation name. (Don't forget to update the id in `package.json`)
2. Edit the slides and optionally preview them locally with `pnpm -F <slide-subfolder-name> run dev` 
3. Commit your changes and push to GitHub
4. Wait for the `deploy` workflow to finish
5. View it at [https://slides.benx.dev/<slide-subfolder-name>](https://slides.benx.dev/)
6. That's it!
