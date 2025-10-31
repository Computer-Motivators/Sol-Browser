# Sol Browser Rebranding Implementation Guide

This document outlines the comprehensive changes needed to complete the Sol Browser rebranding from BrowserOS. This is a living document that tracks progress and provides implementation guidance.

## Executive Summary

Sol Browser is a complete rebrand of BrowserOS for Computer Motivators. This involves:
- **Scope**: ~500+ files across build system, Chromium patches, resources, and documentation
- **Effort**: Estimated 80-120 hours of development work
- **Complexity**: High - requires deep Chromium knowledge
- **Status**: **In Progress** - Core build system complete, Chromium patches pending

## Implementation Status

### ✅ Completed (Phase 1)

#### Build System Core
- [x] Updated `build/context.py` - Changed NXTSCAPE_APP_BASE_NAME to "Sol Browser"
- [x] Updated `build/modules/string_replaces.py` - All branding replacements
- [x] Updated `build/modules/package_linux.py` - Linux packaging with Sol Browser
- [x] Updated `build/modules/package.py` - macOS DMG packaging  
- [x] Updated `build/modules/package_windows.py` - Windows installer packaging
- [x] Updated `build/config/copy_resources.yaml` - Server binary paths
- [x] Updated `build/config/release.linux.yaml` - Build configuration

#### Documentation
- [x] Created new technical `README.md`
- [x] Updated `CONTRIBUTING.md` with Computer-Motivators references
- [x] Created comprehensive `SECURITY.md`
- [x] Created detailed `docs/BUILD.md` compilation guide

#### Extensions
- [x] Removed `resources/files/bug_reporter` extension (feedback removed)
- [x] Updated `resources/files/ai_side_panel/manifest.json` branding

### 🚧 In Progress (Phase 2-7)

The following sections detail what still needs to be done.

## Detailed Implementation Tasks

### Phase 2: Chromium Patches & Core Branding

#### Priority: HIGH | Effort: HIGH (40-60 hours)

These are Chromium source modifications that need updating:

#### A. Settings Pages Branding

**Files to Update:**
```
chromium_patches/chrome/browser/resources/settings/browseros_prefs_page/
  - browseros_prefs_page.html → solbrowser_prefs_page.html
  - Rename directory to solbrowser_prefs_page
  - Update all "BrowserOS" text to "Sol Browser"
  - Update CSS classes from .browseros to .solbrowser

chromium_patches/chrome/browser/resources/settings/nxtscape_page/
  - Rename to sol_page or remove if LLM Hub
  - Update "Nxtscape" references
  
chromium_patches/chrome/browser/resources/settings/settings_menu/settings_menu.html
  - Change menu text "BrowserOS Settings" to "Sol Browser Settings"
  - Update href="/browseros-settings" to "/solbrowser-settings"

chromium_patches/chrome/browser/resources/settings/settings_main/settings_main.html
  - Update showPages_.browseros to showPages_.solbrowser
  - Update component tags
```

**Implementation:**
```bash
# Rename directories
cd packages/browseros/chromium_patches/chrome/browser/resources/settings/
mv browseros_prefs_page solbrowser_prefs_page
mv nxtscape_page sol_page

# Update content with sed (verify each change!)
find . -name "*.html" -exec sed -i 's/BrowserOS/Sol Browser/g' {} \;
find . -name "*.html" -exec sed -i 's/browseros/solbrowser/g' {} \;
find . -name "*.html" -exec sed -i 's/Nxtscape/Sol/g' {} \;
```

#### B. URL Schema Changes (chrome:// to sol://)

**Critical Files:**
```
chromium_patches/chrome/common/url_constants.*
chromium_patches/components/url_formatter/
```

**Changes Required:**
1. Update URL scheme registration
2. Change internal page prefixes
3. Update all references from chrome:// to sol://

**Examples:**
- `chrome://settings` → `sol://settings`
- `chrome://extensions` → `sol://extensions`
- `chrome://version` → `sol://version`

**Implementation Complexity**: HIGH - Requires understanding Chromium's URL routing

#### C. New Tab Page

**Files:**
```
chromium_patches/chrome/browser/ui/webui/ntp/
chromium_patches/chrome/common/url_constants.cc
```

**Required Changes:**
1. Change default new tab URL to `https://computermotivators.com/app/sol`
2. Remove existing new tab page implementation
3. Update new tab constants

**Code Example:**
```cpp
// In url_constants.cc
const char kChromeUINewTabURL[] = "https://computermotivators.com/app/sol";
```

#### D. Default Search Engine

**Files:**
```
chromium_patches/components/search_engines/
chromium_patches/chrome/browser/search_engines/template_url_prepopulate_data.cc
```

**Changes:**
1. Set Sol as default search provider
2. Update search URL to `https://computermotivators.com/app/sol?q={searchTerms}`
3. Remove other default search engines
4. Add SearXNG: `https://search.computermotivators.com/search?q={searchTerms}`

**Implementation:**
```cpp
// In template_url_prepopulate_data.cc
PrepopulatedEngine sol_search = {
  u"Sol",
  u":s",  // keyword
  "https://computermotivators.com/app/sol/favicon.ico",
  "https://computermotivators.com/app/sol?q={searchTerms}",
  // ... other fields
};
```

### Phase 3: Extensions & Features Removal

#### Priority: MEDIUM | Effort: MEDIUM (15-25 hours)

#### A. Remove Feedback Extension ✅
**Status**: DONE - bug_reporter removed

#### B. Remove LLM Hub

**Files to Remove:**
```
chromium_patches/chrome/browser/resources/settings/nxtscape_page/
  (if this is LLM Hub - verify first)
```

**Related Code:**
- Remove LLM Hub menu items
- Remove IDS_CLASH_OF_GPTS_TITLE string
- Remove multi-LLM comparison features

#### C. Simplify LLM Chat

**Current**: Multiple LLM providers
**Target**: Only Sol at https://computermotivators.com/app/sol

**Files:**
```
chromium_patches/chrome/browser/ui/views/side_panel/third_party_llm/
```

**Changes:**
1. Remove provider selection UI
2. Hard-code Sol URL
3. Remove provider settings
4. Simplify to single iframe/webview

#### D. Agent Settings - SearXNG Only

**File:**
```
chromium_patches/chrome/browser/resources/settings/nxtscape_page/
```

**Changes:**
1. Remove all search providers except SearXNG
2. Set default to https://search.computermotivators.com
3. Remove provider selection dropdown

#### E. Remove Default LLM Provider

**Related Settings:**
- Remove pre-configured API keys
- Remove default provider selection
- Require user to add their own

#### F. Unpin Extensions by Default

**File:**
```
chromium_patches/chrome/browser/extensions/extension_action_manager.cc
```

**Change:**
- Set `pinned_to_toolbar` = false for all extensions
- User must manually pin if desired

### Phase 4: UI/UX Changes

#### Priority: LOW | Effort: VERY HIGH (30-40 hours)

**Note**: These are complex UI changes requiring significant Chromium knowledge.

#### A. Design Language Integration

**Dependencies to Add:**
```
# In relevant HTML files
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
<link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
```

**CSS Variables to Add:**
```css
:root {
  --sol-primary: #8E3833;
  --sol-primary-hover: #762E2A;
  --sol-bg-light: #F8F9FA;
  --sol-bg-dark: #2c2c2c;
  --sol-border: #dee2e6;
  --sol-text: #212529;
}
```

#### B. Center Omnibox

**File:**
```
chromium_patches/chrome/browser/ui/views/frame/browser_view.cc
chromium_patches/chrome/browser/ui/views/location_bar/location_bar_view.cc
```

**Required:**
- Modify layout manager to center omnibox
- Adjust toolbar flex properties
- Test responsive behavior

#### C. Floating Tab Indicator

**File:**
```
chromium_patches/chrome/browser/ui/views/tabs/tab.cc
chromium_patches/chrome/browser/ui/views/tabs/tab_style_views.cc
```

**Design:**
- Rounded rectangle background
- Separated from tab content (floating effect)
- Drop shadow for depth
- Animation on tab switch

**CSS Example:**
```css
.active-tab-indicator {
  border-radius: 12px;
  background: var(--sol-primary);
  box-shadow: 0 2px 8px rgba(0,0,0,0.15);
  margin: 4px;
  transition: all 0.2s ease;
}
```

#### D. Reorganize "..." Menu

**File:**
```
chromium_patches/chrome/browser/ui/views/toolbar/app_menu.cc
```

**Current Structure:**
- Flat list of many options
- No categorization

**New Structure:**
```
Tools
  ├── Extensions
  ├── Downloads
  └── History

Settings
  ├── Preferences
  ├── Sol Browser Settings
  └── Privacy

Advanced
  ├── Developer Tools
  ├── Task Manager
  └── Clear Browsing Data

Help
  └── About Sol Browser
```

### Phase 5: Privacy & Security

#### Priority: HIGH | Effort: MEDIUM (20-30 hours)

#### A. Remove Tracking & Analytics ✅

**Documentation**: Created SECURITY.md

**Code Changes Needed:**
```
chromium_patches/chrome/browser/metrics/
  - Disable all metrics collection
  
chromium_patches/components/metrics/
  - Remove UMA (User Metrics Analysis)
  
chromium_patches/chrome/browser/safe_browsing/
  - Disable Safe Browsing telemetry (keep local checks)
  
chromium_patches/components/rappor/
  - Remove RAPPOR privacy reporting
  
chromium_patches/components/variations/
  - Disable field trials reporting
```

**Files to Modify:**
```cpp
// In metrics_service.cc
void MetricsService::Start() {
  // Comment out or return early
  return;  // Disable metrics collection for Sol Browser
}

// In safe_browsing_service.cc
void SafeBrowsingService::SendReport() {
  // Disable telemetry
  return;
}
```

#### B. Audit Telemetry Code

**Systematic Approach:**
```bash
# Find all telemetry-related code
grep -r "UMA_HISTOGRAM" chromium_patches/
grep -r "base::UmaHistogram" chromium_patches/
grep -r "metrics::Record" chromium_patches/
grep -r "RecordAction" chromium_patches/
```

**For each occurrence:**
1. Determine if user-facing feature depends on it
2. If not, remove or disable
3. If yes, make opt-in only

### Phase 6: Logo & Assets

#### Priority: HIGH | Effort: LOW (5-10 hours)

#### A. Download Logos

**URLs:**
- Rounded: https://computermotivators.com/web/image/1140-2f4e7afe/Logo.webp
- Rectangular: https://computermotivators.com/web/image/1937-53f2964c/Navbar.png

**Implementation:**
```bash
cd packages/browseros/resources/icons/

# Download logos
curl -o cm_logo_rounded.webp https://computermotivators.com/web/image/1140-2f4e7afe/Logo.webp
curl -o cm_logo_rect.png https://computermotivators.com/web/image/1937-53f2964c/Navbar.png

# Convert to required formats
# BrowserOS uses SVG, but Sol Browser will use PNG/WEBP
# Need to create multiple sizes:
# - 16x16, 32x32, 48x48, 64x64, 128x128, 256x256, 512x512

# Use ImageMagick or similar
for size in 16 32 48 64 128 256 512; do
  convert cm_logo_rounded.webp -resize ${size}x${size} product_logo_${size}.png
done
```

#### B. Update Icon References

**Files:**
```
resources/icons/
  ├── product_logo.png
  ├── product_logo_16.png
  ├── product_logo_32.png
  ├── product_logo_64.png
  ├── product_logo_128.png
  ├── product_logo_256.png
  ├── linux/
  ├── mac/
  └── win/
```

**Platform-Specific:**
- **Linux**: Update .desktop icon, window icon
- **macOS**: Update .app bundle icon (ICNS format)
- **Windows**: Update .exe icon, installer icon

#### C. Convert SVG References to PNG/WEBP

**Chromium Code Changes:**

Most Chromium code expects vector formats or specific image formats. Need to:
1. Update image loading code to handle PNG/WEBP
2. Ensure proper scaling for HiDPI displays
3. Test on all platforms

**Files to Update:**
```
chromium_patches/chrome/app/theme/chromium/
chromium_patches/chrome/browser/ui/views/frame/browser_frame.cc
chromium_patches/chrome/browser/ui/views/frame/opaque_browser_frame_view.cc
```

### Phase 7: Build & Distribution

#### Priority: HIGH | Effort: LOW (Already mostly done)

#### A. AppImage Configuration ✅
**Status**: DONE in package_linux.py

#### B. Test Build Process

**Checklist:**
- [ ] Linux x64 build succeeds
- [ ] Linux ARM64 build succeeds
- [ ] macOS x64 build succeeds
- [ ] macOS ARM64 build succeeds
- [ ] macOS Universal build succeeds
- [ ] Windows x64 build succeeds
- [ ] All packages install correctly
- [ ] Browser launches and shows Sol Browser branding
- [ ] Extensions load correctly
- [ ] Settings pages work
- [ ] New tab goes to Sol

**Test Command:**
```bash
# Full build and package test
python build/build.py \
  --config build/config/release.linux.yaml \
  --chromium-src ~/chromium/src \
  --build --package

# Verify package
file dist/*/SolBrowser.AppImage
./dist/*/SolBrowser.AppImage --version
```

## Implementation Priority Order

Recommended implementation order:

1. **Logo & Assets** (Week 1)
   - Quick win, visible impact
   - Download and convert logos
   - Update icon paths

2. **Settings Pages Branding** (Week 1-2)
   - Rename directories
   - Update HTML/CSS files
   - Test settings UI

3. **URL Schema Changes** (Week 2-3)
   - Complex but critical
   - Change chrome:// to sol://
   - Extensive testing required

4. **New Tab & Search** (Week 3)
   - Change new tab URL
   - Set Sol as default search
   - Add SearXNG

5. **Extension Cleanup** (Week 3-4)
   - Remove LLM Hub
   - Simplify LLM Chat
   - Configure agent settings

6. **Privacy & Security** (Week 4-5)
   - Audit and remove analytics
   - Disable telemetry
   - Test data collection

7. **UI/UX Changes** (Week 5-7)
   - Most time-consuming
   - Center omnibox
   - Floating tab indicator
   - Menu reorganization

8. **Testing & Polish** (Week 7-8)
   - Full system tests
   - Package creation
   - Bug fixes

## Testing Strategy

### Unit Tests
- Test individual branding changes
- Verify logo loading
- Check URL schema changes

### Integration Tests
- Full build pipeline
- Package installation
- Browser launch

### Manual Testing
- Navigate through all settings
- Test Sol search
- Verify new tab page
- Check all internal URLs (sol://)
- Test extensions
- Verify no data sent to Google/BrowserOS

## Risks & Challenges

### Technical Risks

1. **Chromium Version Compatibility**
   - Risk: Patches may not apply to newer Chromium
   - Mitigation: Test with current Chromium version, prepare for merge conflicts

2. **URL Schema Breaking Changes**
   - Risk: Changing chrome:// to sol:// may break extensions
   - Mitigation: Maintain compatibility layer during transition

3. **Build System Complexity**
   - Risk: Chromium build is complex, errors hard to debug
   - Mitigation: Incremental changes, test frequently

4. **Icon Format Issues**
   - Risk: PNG/WEBP may not work where SVG expected
   - Mitigation: Test on all platforms, may need to recreate as SVG

### Resource Risks

1. **Time Estimate Accuracy**
   - Risk: Actual time may exceed estimates
   - Mitigation: Built in 50% buffer

2. **Chromium Expertise**
   - Risk: Deep Chromium knowledge required
   - Mitigation: Reference Chromium documentation, community

## Success Criteria

Project is complete when:

- [ ] All "BrowserOS" text changed to "Sol Browser"
- [ ] Computer Motivators logo displays throughout UI
- [ ] sol:// URL schema works for all internal pages
- [ ] New tab opens to https://computermotivators.com/app/sol
- [ ] Default search uses Sol
- [ ] No tracking/analytics/telemetry
- [ ] Builds succeed on all platforms
- [ ] Packages install and run correctly
- [ ] Documentation is complete and accurate
- [ ] Security policy documented

## Resources

### Documentation
- Chromium Developer Docs: https://www.chromium.org/developers/
- Chromium Source: https://source.chromium.org/
- GN Reference: https://gn.googlesource.com/gn/+/main/docs/reference.md

### Tools
- Chromium Code Search: https://source.chromium.org/chromium
- GN Quickstart: https://gn.googlesource.com/gn/+/main/docs/quick_start.md
- depot_tools: https://commondatastorage.googleapis.com/chrome-infra-docs/flat/depot_tools/docs/html/depot_tools.html

### Community
- Chromium Dev: https://groups.google.com/a/chromium.org/g/chromium-dev
- Chromium Extensions: https://groups.google.com/a/chromium.org/g/chromium-extensions

## Revision History

- **2025-10-29**: Initial implementation guide created
- **Status**: Phase 1 (Build System) complete, Phases 2-7 documented

---

**This is a living document. Update as implementation progresses.**

Created by Computer Motivators
