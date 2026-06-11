# Jianchuan Chen's Homepage

Personal academic homepage of Jianchuan Chen, researcher at Alibaba Group.  
Live site: [janaldochen.github.io](https://janaldochen.github.io)

## Local Development

**Requirements:** Ruby + Bundler

```bash
# Install dependencies
bundle install

# Start local server (http://localhost:4000)
bundle exec jekyll serve
```

## Adding a New Publication

1. Add a teaser image to `images/`
2. Edit `_pages/about.md` — add a paper card in the Publications section
3. Update the News section in `_pages/about.md`

## Site Configuration

| File | Purpose |
|------|---------|
| `_config.yml` | Author info, social links, site metadata |
| `_data/navigation.yml` | Navigation menu items |
| `_pages/about.md` | All homepage content |
| `images/` | Paper teasers and author avatar |
