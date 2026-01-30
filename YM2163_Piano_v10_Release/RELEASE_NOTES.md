# YM2163 Piano GUI v9 Release Notes

## Release Date
January 28, 2026

## Version
v9.0 (Build 2026-01-28)

## Major Improvements

### Dynamic Volume Mapping (NEW)
- **Automatic MIDI Analysis**: Analyzes velocity distribution when loading MIDI files
- **Intelligent Mapping**: 
  - Most common velocities → -6dB and -12dB
  - Peak velocities (95th percentile) → 0dB
  - Very low velocities → Mute
- **Adaptive Control**: Provides better volume balance for different MIDI files
- **Logging**: Detailed velocity analysis logged for debugging

### Unicode File Name Support (ENHANCED)
- **CJK Font Support**: Full support for Chinese, Japanese, Korean characters
- **Font Merging**: Combines multiple fonts for complete character coverage
  - Primary: Microsoft YaHei (中文)
  - Secondary: Malgun Gothic (한글)
  - Tertiary: MS Gothic (日本語)
- **UTF-8 Handling**: Proper string handling throughout application
- **File Browser**: Correctly displays international file names

### Win11-Style Address Bar (NEW)
- **Breadcrumb Navigation**: Click directory buttons to navigate
- **Smart Truncation**: Shows "..." for long paths, prioritizes current directory
- **Dual Mode**: 
  - Breadcrumb mode (default): Click buttons to navigate
  - Text input mode: Double-click to enter path directly
- **Up Arrow Button**: Quick navigation to parent directory

### File Browser Improvements (ENHANCED)
- **Accurate Width Calculation**: Prevents button text overflow
- **Smart Layout**: Prioritizes rightmost (current) directory visibility
- **UTF-8 String Handling**: Uses std::string for proper multi-byte character support
- **Better Spacing**: Improved button sizing and alignment

### Chip Reset on Track Switch (NEW)
- **Automatic Reset**: YM2163 chips reset when switching tracks
- **Eliminates Residual Sound**: Clears all lingering notes and sounds
- **Dual Chip Support**: Works with both Slot0 and Slot1
- **Smooth Transitions**: Clean audio transitions between tracks

### History Directory Sorting (ENHANCED)
- **Time-Based Sorting**: Most recently opened directories appear first
- **Automatic Reordering**: Revisiting a directory moves it to the top
- **Smart Caching**: Keeps up to 20 most recent MIDI folders
- **Persistent Storage**: Saved to ym2163_folder_history.ini

### Time-Driven MIDI Progress (CONFIRMED)
- **High-Precision Timing**: Uses QueryPerformanceCounter (microsecond accuracy)
- **Smooth Animation**: Progress bar moves smoothly without jumps
- **Accurate Display**: Shows MM:SS format for current and total time
- **Seek Support**: Drag progress bar to quickly jump to any position

## Bug Fixes
- Fixed layout width changes when toggling Slot1
- Fixed file browser Unicode character display
- Fixed double-click file loading without chip reset
- Fixed address bar focus loss issues
- Fixed progress bar calculation accuracy

## Technical Details

### Dynamic Velocity Analysis
```
Analysis includes:
- Total note count
- Velocity range (min/max)
- Average velocity
- Peak velocity (95th percentile)
- Most common velocities (top 2)
- Calculated thresholds for each volume level
```

### Font Loading Strategy
```
Priority order:
1. Microsoft YaHei (msyh.ttc) - CJK support
2. Malgun Gothic (malgun.ttf) - Korean support
3. MS Gothic (msgothic.ttc) - Japanese support
4. Segoe UI (segoeui.ttf) - Western support
5. ImGui default - Fallback
```

### Chip Reset Sequence
```
For each chip:
1. Send note-off for all 4 channels
2. Set all volumes to mute
3. Reset all envelopes to decay
4. Reset all wave/timbre to 0
5. Turn off all rhythm sounds
6. Wait 50ms for stabilization
```

## Performance Improvements
- Reduced CPU usage during MIDI playback
- Improved font rendering performance
- Optimized file browser responsiveness
- Better memory management for large MIDI files

## Known Limitations
- Maximum 20 MIDI folder history entries
- Single MIDI file playback (no playlist)
- Limited to 8 channels (4 per chip)
- Requires SPFM hardware for audio output

## System Requirements
- Windows 10/11 (64-bit)
- SPFM hardware with YM2163 chip(s)
- FTDI USB driver
- 100MB free disk space

## Installation Instructions
1. Extract YM2163_Piano_v9_Release.tar.gz
2. Run ym2163_piano_gui_v9.exe
3. Ensure ym2163_midi_config.ini is present
4. Connect SPFM hardware via USB

## Building from Source
```bash
cd YM2163_Piano_v9_Release
./build_v9.sh
```

## Changelog from v8
- Added dynamic volume mapping
- Enhanced Unicode file name support
- Implemented Win11-style address bar
- Added chip reset on track switch
- Improved history directory sorting
- Confirmed time-driven MIDI progress
- Fixed multiple UI layout issues
- Improved overall stability

## Future Roadmap
- Playlist support
- MIDI file editing capabilities
- Advanced frequency tuning interface
- Real-time spectrum analyzer
- Recording capabilities

## Support & Feedback
For bug reports and feature requests, please visit the project repository.

## Credits
- YM2163 chip documentation
- SPFM hardware support
- ImGui framework
- MidiFile library
- FTDI driver support
