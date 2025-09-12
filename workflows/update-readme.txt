name: Update README with Random Quote

on:
  schedule:
    - cron: '0 * * * *'   # runs every hour
  workflow_dispatch:

jobs:
  update:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2

      - name: Pick random quote
        id: get_quote
        run: |
          QUOTE=$(shuf -n 1 quotes.txt)
          echo "quote=$QUOTE" >> $GITHUB_OUTPUT

      - name: Update README.md
        run: |
          sed -i "s|<div id=\"cryptic-quote\">.*</div>|<div id=\"cryptic-quote\">${{ steps.get_quote.outputs.quote }}</div>|g" README.md

      - uses: stefanzweifel/git-auto-commit-action@v4
        with:
          commit_message: "chore: update random quote"
