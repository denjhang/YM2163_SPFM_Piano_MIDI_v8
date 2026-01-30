# YM2163 Piano GUI - Complete Version History

## Version Timeline

**v1.0** → **v2.0** → **v3.0** → **v4.0** → **v5.0** → **v6.0** → **v7.0** → **v8.0** → **v9.0**

---

## v9.0 (2026-01-28) - Current Release

### Major Features
- ✨ **Dynamic Drum Allocation System**: Drums alternate between two chips in dual-chip mode
- ✨ **File Browser Highlight System**: Yellow highlight for exited folders, blue for playing paths
- ✨ **Filename Scrolling Display**: Auto-scrolling for long filenames on hover/select
- ✨ **Scroll Position Memory**: Remembers scroll position when navigating folders
- ✨ **Long Path Support**: Supports paths exceeding 260 characters
- ✨ **Win11-Style Address Bar**: Breadcrumb navigation with folder name abbreviation
- ✨ **Dynamic Volume Mapping**: Intelligent MIDI velocity analysis and mapping

### Improvements
- 🔧 Progress bar seek optimization (fixed no response/burst notes issue)
- 🔧 Track switch optimization (clears residual piano keys)
- 🔧 Enhanced Unicode filename support
- 🔧 Single-click unified file browser operation
- 🔧 Improved error messages for file loading

### Bug Fixes
- 🐛 Fixed drum UI showing on both chips simultaneously
- 🐛 Fixed progress bar seek issues
- 🐛 Fixed piano key residual when switching tracks
- 🐛 Fixed folder highlight flashing
- 🐛 Fixed scroll position not restored
- 🐛 Fixed long path loading errors

---

## v8.0 (2026-01-27)

### Features
- MIDI playback with high-precision timing
- Dual YM2163 chip support (8 channels total)
- Real-time piano keyboard visualization
- Channel status display
- Velocity mapping support
- Sustain pedal support

### Improvements
- Improved MIDI event processing
- Better channel allocation
- Enhanced UI responsiveness

---

## v7.0 (2026-01-26)

### Features
- File browser with navigation history
- MIDI folder history
- Auto-play next track
- Sequential/random playback modes
- Global media key support

### Improvements
- Win11-style UI design
- Better file organization
- Improved navigation

---

## v6.0 (2026-01-25)

### Features
- Unicode path support
- CJK font support
- Multi-language file names
- Improved file browser

### Bug Fixes
- Fixed Unicode display issues
- Fixed path handling bugs

---

## v5.0 (2026-01-22)

### Features
- MIDI player integration
- Progress bar with seek support
- Time display (MM:SS format)
- Play/Pause/Stop controls

### Improvements
- Better MIDI timing
- Smooth progress animation

---

## v4.0 - v4.5 (2026-01-20 to 2026-01-21)

### Features (v4.0)
- Dual chip support
- Channel allocation system
- Envelope control
- Wave/timbre selection

### Improvements (v4.1-v4.5)
- High DPI support
- Crash fixes
- Frequency table improvements
- UI refinements

---

## v3.0 (2026-01-19)

### Features
- Piano keyboard interface
- Real-time note visualization
- Octave control
- Volume control

### Improvements
- Better keyboard mapping
- Improved visual feedback

---

## v2.0 (2026-01-18)

### Features
- Basic GUI with ImGui
- Note playback
- Simple controls

---

## v1.0 (2026-01-17)

### Features
- Initial release
- Basic YM2163 communication
- Simple note playback

---

## Feature Evolution Summary

### Audio Engine
- v1.0: Basic note playback
- v3.0: Piano keyboard interface
- v4.0: Dual chip support
- v5.0: MIDI playback
- v8.0: High-precision timing
- v9.0: Dynamic drum allocation

### File Handling
- v5.0: Basic file loading
- v6.0: Unicode support
- v7.0: File browser with history
- v9.0: Long path support, scrolling filenames

### User Interface
- v2.0: Basic ImGui interface
- v3.0: Piano keyboard visualization
- v4.0: Channel status display
- v7.0: Win11-style design
- v9.0: Advanced highlighting, scrolling text

### MIDI Features
- v5.0: Basic MIDI playback
- v6.0: Better timing
- v8.0: Velocity mapping
- v9.0: Dynamic volume mapping, precise seeking

---

## Statistics

- **Total Versions**: 9 major releases
- **Development Period**: January 17-28, 2026 (12 days)
- **Total Features Added**: 50+
- **Total Bug Fixes**: 30+
- **Code Size**: ~4000 lines → ~3800 lines (optimized)
- **Supported Formats**: MIDI (.mid, .midi)
- **Supported Chips**: YM2163 (1-2 chips, 4-8 channels)

---

## Key Milestones

1. **v1.0**: First working prototype
2. **v3.0**: Piano keyboard interface
3. **v4.0**: Dual chip support breakthrough
4. **v5.0**: MIDI player integration
5. **v6.0**: Unicode support
6. **v7.0**: Complete file browser
7. **v8.0**: Production-ready release
8. **v9.0**: Feature-complete with advanced UI

---

## Technical Achievements

### Performance
- High-precision timing (microsecond accuracy)
- Efficient channel allocation
- Smooth UI rendering
- Low CPU usage

### Compatibility
- Windows 10/11 support
- Unicode/CJK support
- Long path support
- High DPI support

### User Experience
- Intuitive interface
- Responsive controls
- Visual feedback
- Error handling

---

## Future Development

### Planned Features
- Playlist support
- MIDI editing
- Recording capabilities
- More chip support
- Advanced tuning interface
- Spectrum analyzer

### Potential Improvements
- Performance optimization
- More file formats
- Cloud integration
- Mobile version

---

**Project Start**: January 17, 2026
**Current Version**: v9.0
**Last Updated**: January 28, 2026 23:53
**Status**: Active Development
