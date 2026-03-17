# Your Security Blog

A clean, minimal security research blog template — inspired by the AcademicPages/Minimal Mistakes Jekyll theme style.

## Structure

```
blog/
├── index.html          # Homepage
├── 
├── thoughts.html       # Thoughts listing
├── writeups.html       # Writeups listing
├── about.html          # About page
├── css/
│   └── style.css       # All styles
├── images/
│   └── avatar.png      # Your profile photo (replace this!)
└── posts/
    └── all-it-took-was-null.html   # Your first blog post
```

## How to customize

1. **Replace placeholders**: Search for `0xWilayah`, `Your Name`, `handle`, `your@email.com`, `yourprofile`, `yourhandle` across all HTML files and replace with your info.

2. **Add your avatar**: Replace `images/avatar.png` with your profile photo (ideally square, at least 320x320px).

3. **Add new posts**: 
   - Copy `posts/all-it-took-was-null.html` as a template
   - Edit the content
   - Add a link to it in the appropriate listing page (research.html, thoughts.html, or writeups.html)

## How to deploy on GitHub Pages

1. Create a repo named `yourusername.github.io`
2. Push all these files to the `main` branch
3. Go to Settings > Pages > Source: Deploy from branch (main)
4. Your site will be live at `https://yourusername.github.io`

## Adding more posts

Each listing page (research.html, thoughts.html, writeups.html) has commented-out template blocks you can copy. Just uncomment and fill in your details.

## Notes

- Pure HTML/CSS — no build tools, no dependencies, no frameworks
- Mobile responsive
- Easy to extend and customize
- Just edit and push — that's it
