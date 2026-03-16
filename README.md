# Custom Button Panel Driver for Control4

A powerful and flexible web-based button panel driver for Control4 systems, featuring customizable pages, LED state feedback, icon support, and multiple themes.

![Version](https://img.shields.io/badge/version-2.2-blue)
  <img src="https://img.shields.io/badge/Control4-OS%203.x-red" alt="Control4 OS 3.x">
  <img src="https://img.shields.io/badge/Control4-OS%204.x-blue" alt="Control4 OS 4.x">

## Features

###  Flexible Button Layout
- **48 buttons** organized across 4 swipeable pages
- **12 buttons per page** with customizable grid layout
- **Strict page boundaries** - buttons stay on their designated pages
- **Individual page routing** - access specific pages via URL
- **Custom page titles** - unique title for each page

###  Visual Customization
- **7 built-in themes** including Control4 SnapOne (blue accent)
- **4 LED indicator styles**: Left Bar, Dot, Border, Background
- **Icon support**: Material Design Icons (MDI), Emojis, Custom image URLs
- **Responsive design** optimized for tablets and touchscreens
- **Touchscreen-friendly** large buttons with visual feedback

###  Real-Time Updates
- **Live state feedback** - LED indicators show On/Off state
- **Auto-refresh** every 5 seconds maintains current state
- **Persistent position** - stays on your current page during refresh
- **Bidirectional control** - updates when devices change externally (with programming)

###  Multi-Tablet Support
- **Page routing** - different tablets show different pages
- **Custom titles per page** - e.g., "KITCHEN", "LIVING ROOM"
- **Flexible button counts** - use 1-12 buttons per page as needed
- **Server-side filtering** - clean, efficient page separation

## Screenshots

<img width="1563" height="976" alt="image" src="https://github.com/user-attachments/assets/eb24b7cd-2b7e-468e-b718-0a6ae2e484c1" />
<img width="1562" height="977" alt="image" src="https://github.com/user-attachments/assets/35f9a17b-c764-4e9c-bbbb-893cb3f72e53" />
<img width="1564" height="979" alt="image" src="https://github.com/user-attachments/assets/5abc2519-8673-4135-9147-ffc16610e448" />
<img width="1613" height="1044" alt="image" src="https://github.com/user-attachments/assets/892f4d34-d71b-47b3-a60f-c77e266d88b8" />


## Installation

### Requirements
- Control4 OS 3.x or later
- Network-connected Control4 controller
- Web browser or tablet for accessing the interface

### Steps

1. **Download** the latest `CustomButtonPanel.c4z` from releases
2. **Open Composer** and navigate to your project
3. **Add Driver**:
   - Right-click in the project tree
   - Select "Add Driver"
   - Choose "Custom Button Panel"
4. **Configure** in Properties (see Configuration section)
5. **Program** button events (see Programming section)

## Configuration

### Basic Setup

1. **HTTP Port** (default: 8080)
   - Port for web interface
   - Change only if port conflict exists

2. **Theme** (default: Control4 SnapOne)
   - Dark Modern
   - Control4 SnapOne (blue accent)
   - Light Modern
   - Blue Professional
   - Green Nature
   - Purple Elegant
   - Custom (define your own colors)

3. **LED Indicator Style** (default: Left Bar)
   - **Left Bar** - 4px blue bar on left edge (recommended)
   - **Dot** - Small dot in top-right corner
   - **Border** - Glowing border around button
   - **Background** - Full button background glow

4. **Show Title** (default: No)
   - Display title on main page
   - Individual pages always show their page title

### Button Configuration

For each button (1-48):

```
Button X Enabled: Yes/No
Button X Text: "Kitchen Lights"
Button X Icon: mdi-lightbulb (or emoji or URL)
Button X State: On/Off
```

### Page Titles

```
Panel Title: BUTTONS (main page)
Page 1 Title: KITCHEN
Page 2 Title: LIVING ROOM
Page 3 Title: BEDROOMS
Page 4 Title: GARAGE
```

### Icon Options

**Material Design Icons:**
```
mdi-lightbulb
mdi-television
mdi-music
mdi-door
mdi-lock
mdi-thermometer
```
[Full MDI icon list](https://pictogrammers.com/library/mdi/)

**Emojis:**
```
💡 (lightbulb)
🚪 (door)
🔒 (lock)
🌡️ (thermometer)
☕ (coffee)
```

**Custom Images:**
```
http://example.com/icon.png
https://example.com/icon.jpg
```

## Usage

### Accessing the Interface

**Main Page (all buttons with swiping):**
```
http://[controller-ip]:8080/
http://[controller-ip]:8080/index.html
```

**Individual Pages (no swiping, custom titles):**
```
http://[controller-ip]:8080/page1.html  - Buttons 1-12
http://[controller-ip]:8080/page2.html  - Buttons 13-24
http://[controller-ip]:8080/page3.html  - Buttons 25-36
http://[controller-ip]:8080/page4.html  - Buttons 37-48
```

### Multi-Room Setup Example

**Kitchen Tablet:**
```
URL: http://192.168.1.100:8080/page1.html
Title: KITCHEN
Buttons: 1-9 (kitchen controls)
```

**Living Room Tablet:**
```
URL: http://192.168.1.100:8080/page2.html
Title: LIVING ROOM
Buttons: 13-20 (living room controls)
```

**Bedroom Tablets:**
```
URL: http://192.168.1.100:8080/page3.html
Title: BEDROOMS
Buttons: 25-30 (bedroom controls)
```

## Programming

### Basic Button Press

```
When: Button 1 Pressed
Actions:
  → Lighting > Kitchen Lights > Toggle
```

### Button with State Feedback

```
Programming Set 1: Button Press
When: Button 1 Pressed
Actions:
  → Lighting > Kitchen Lights > Toggle
  → If Lights On:
      → Custom Button Panel > Set Button 1 State > On
  → Else:
      → Custom Button Panel > Set Button 1 State > Off

Programming Set 2: Device Monitoring (CRITICAL for sync)
When: Kitchen Lights > State Changed
Actions:
  → If: Level > Greater than 0
      → Custom Button Panel > Set Button 1 State > On
  → Else:
      → Custom Button Panel > Set Button 1 State > Off
```

**Note:** The second programming set ensures the button state updates even when the light is controlled via:
- Physical switch
- Voice control
- Mobile app
- Schedules/timers

### Dynamic Label Update

```
When: Temperature Changed
Actions:
  → Custom Button Panel > Set Button 5 Text > "Temp: [temperature]°F"
```

## Button Ranges

Buttons are organized into strict page ranges:

| Page | Button Range | URL |
|------|-------------|-----|
| Page 1 | 1-12 | /page1.html |
| Page 2 | 13-24 | /page2.html |
| Page 3 | 25-36 | /page3.html |
| Page 4 | 37-48 | /page4.html |

**Important:** Buttons respect these ranges even if you disable some buttons. For example, if you only enable buttons 1-9 on page 1, button 13 will still appear on page 2, not page 1.

## Advanced Features

### Server-Side Filtering

Individual pages (`page1.html`, etc.) use server-side filtering for efficient performance:
- Server sends only buttons for requested page
- Reduces bandwidth and browser processing
- Faster load times on tablets

### Auto-Refresh with Position Memory

- Refreshes button states every 5 seconds
- Remembers which page you're viewing
- Updates states without jumping to page 1
- Seamless experience across all pages

### Debug Logging

Enable in Composer Properties:

```
Debug Mode: Print and Log
```

**Auto-disable:** Debug mode automatically turns off after 5 hours to prevent log buildup.

**What gets logged:**
- Driver initialization
- Button presses
- State changes
- Property updates
- HTTP requests

## Themes

### Control4 SnapOne (Default)
- Deep black background (#0a0a0a)
- Blue accent (#4a9eff)
- Professional appearance
- Matches Control4 aesthetic

### Dark Modern
- Dark background
- Orange accent
- High contrast

### Light Modern
- Light gray background
- Blue accent
- Easy on eyes in bright rooms

### Custom Theme
Define your own colors:
- Custom Background Color
- Custom Button Color
- Custom Text Color
- Custom Accent Color

## Troubleshooting

### Driver Won't Load
- Check Composer logs for errors
- Verify driver version compatibility
- Ensure no port conflicts (default 8080)

### Buttons Not Responding
- Verify button is enabled in Properties
- Check debug logs for button press events
- Ensure programming is set up correctly

### LED States Not Updating
- Confirm "Set Button State" commands are programmed
- Check device monitoring programming (see Programming section)
- Verify auto-refresh is working (should update every 5s)

### Page Jumps to Page 1
- Install latest version (v2.2+)
- Auto-refresh now maintains page position

### Icons Not Displaying
- MDI icons: Ensure proper prefix (mdi-lightbulb)
- Custom images: Verify URL is accessible
- Check browser console for errors

### Browser Compatibility
- Modern browsers (Chrome, Safari, Firefox, Edge)
- Tablets (iOS, Android, Windows)
- Touchscreen optimized
- Responsive design (320px - 1920px)

## Version History

### v2.2 (Current)
- Added page routing with custom titles
- Implemented strict page boundaries
- Fixed auto-refresh page jumping
- Added server-side filtering
- Enhanced debug logging
- Changed Control4 theme to blue accent
- Added Left Bar LED indicator style
- Memory leak audit and cleanup

### v2.1
- Added LED state feedback
- Implemented 4 LED indicator styles
- Enhanced icon support
- Added 7 themes

### v2.0
- Initial release with 48 buttons
- Basic page swiping
- Icon support

## Related Projects
### APC AP8930 PDU Driver
[https://github.com/ckunakdot/APC_AP8930_PDU](https://github.com/ckunakdot/APC_AP8930_PDU)

Use the Custom Button Panel to control APC PDU outlets:
- Create dedicated buttons for each outlet
- LED indicators show outlet On/Off state
- Quick access to power cycling equipment
- Organize by equipment type or location across pages

**Example Setup:**
```
Page 1 Title: RACK POWER
Button 1: AV Rack Outlet 1 (Amplifier)
Button 2: AV Rack Outlet 2 (Receiver)
Button 3: Network Outlet 1 (Switch)
Button 4: Network Outlet 2 (Router)
...

Programming:
When: Button 1 Pressed
  → APC PDU > Outlet 1 > Toggle
  → If Outlet On:
      → Custom Button Panel > Set Button 1 State > On
  → Else:
      → Custom Button Panel > Set Button 1 State > Off
```

This combination provides a professional web-based control interface for your power distribution system.

### Webview Template
[https://github.com/ckunakdot/Custom-Button-Panel/blob/main/troubleshoot.c4z](https://github.com/ckunakdot/Custom-Button-Panel/blob/main/troubleshoot.c4z)

Use the included `troubleshoot.c4z` as a starting point for creating custom webview drivers:
- Pre-configured to display the Custom Button Panel interface
- Can be modified to fit your specific needs
- Useful for embedding the panel in Control4 navigators
- Easy to customize for different rooms or functions

**How to Use:**
1. Download `troubleshoot.c4z` from the repository
2. Install in Composer alongside the Custom Button Panel driver
3. Modify the webview URL to point to your desired page:
   - Default view: `http://[controller-ip]:8080/`
   - Kitchen page: `http://[controller-ip]:8080/page1.html`
   - Living room: `http://[controller-ip]:8080/page2.html`
4. Add to navigator views for easy access

This allows you to integrate the button panel directly into Control4 touch panels and on-screen displays.

## Support

For issues, feature requests, or questions:
- Open an issue on GitHub
- Check embedded documentation in Composer
- Review troubleshooting section above

