name: Generate snake animation

on:
  schedule: [{ cron: "0 */12 * * *" }]   # every 12 hours
  workflow_dispatch:
  push: { branches: ["main"] }

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Generate snake.svg
        uses: Platane/snk@v3
        with:
          github_user_name: sakshivedi-1
          outputs: |
            dist/snake.svg

      - name: Commit & push
        run: |
          mkdir -p output
          cp dist/snake.svg output/snake.svg
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git add -A
          git commit -m "chore: update snake.svg" || echo "No changes"
          git push
