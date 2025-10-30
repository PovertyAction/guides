#### ABOUT

**⚠️ IMPORTANT: This repository has been deprecated and archived.**

This repository previously hosted materials for IPA & GPRL data cleaning guides. All content has been migrated to a new location.

**For the latest data cleaning guide content, please visit:**
- **New Repository:** https://github.com/PovertyAction/ipa-research-data-science-hub
- **Data Cleaning Guide:** https://github.com/PovertyAction/ipa-research-data-science-hub/tree/main/data-cleaning
- **Published Site:** https://data.poverty-action.org/data-cleaning/

The content in this repository is kept for archival purposes only and will not be updated. Please use the new repository for any contributions or updates.

---

#### LEGACY INFORMATION

This repository previously hosted materials for IPA & GPRL guides. The materials were accessible on the guides' [website](https://povertyaction.github.io/guides/cleaning/readme/), which now redirects to the new location.

---

## Testing the Redirect Locally

If you want to test the redirect functionality locally before it's deployed to GitHub Pages, you can install Jekyll and run the site on your machine.

### Installing Jekyll on Windows

1. **Download and Install Ruby**
   - Download Ruby+Devkit from [RubyInstaller Downloads](https://rubyinstaller.org/downloads/)
   - Use the latest stable version (Ruby 3.0+)
   - Run the installer with default options
   - At the end of installation, check "Run 'ridk install'" to set up MSYS2 and development toolchain

2. **Complete MSYS2 Installation**
   - When prompted, choose option `3` (MSYS2 and MINGW development toolchain)
   - Press Enter to install

3. **Install Jekyll and Bundler**
   - Open a new Command Prompt window (important for PATH changes to take effect)
   - Run:
     ```
     gem install jekyll bundler
     ```

4. **Verify Installation**
   - Check Jekyll version:
     ```
     jekyll -v
     ```
   - If you get an error, restart your computer and try again

5. **Install Project Dependencies**
   - Navigate to this repository directory:
     ```
     cd c:\Users\YourUsername\code\guides
     ```
   - Install gems from Gemfile:
     ```
     bundle install
     ```

6. **Run the Site Locally**
   - Start the Jekyll server:
     ```
     bundle exec jekyll serve
     ```
   - Open your browser to: `http://localhost:4000/guides/`
   - You should see the redirect message and be redirected after 5 seconds

### Alternative: Windows Subsystem for Linux (WSL)

If you're on Windows 10 version 1607 or later, you can also install Jekyll via WSL. Follow the [Linux installation instructions](https://jekyllrb.com/docs/installation/ubuntu/) after setting up WSL.
