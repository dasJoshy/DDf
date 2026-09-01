# Der Dümmste fliegt – UI/UX Design Spec & Figma Export

## Project Overview
**Browser-based Party Game UI/UX for Host (Desktop) & Player (Mobile)**

- **Host View**: Desktop (1440px) – question control, answer evaluation, live log, player dashboard
- **Player View**: Mobile (390px) – question display, voting flow, spectate mode
- **Room-based**: No accounts, OTP-based join links with player names pre-filled
- **Game Flow**: Sequential Q&A, voting after each round, life/heart system

---

## Design System

### Color Palette
| Token | Color | Usage |
|-------|-------|-------|
| `--accent` | `#38bdf8` (Sky) | Primary interactive controls |
| `--success` | `#4ade80` (Green) | Correct answers, positive feedback |
| `--danger` | `#f87171` (Red) | Wrong answers, eliminations |
| `--warning` | `#fbbf24` (Amber) | Cautions |
| `--muted` | `#64748b` (Slate) | Skip, secondary actions |
| `--lime` | `#a3e635` (Lime) | Brand color, title, ONE bold visual move |
| `--coral` | `#fb7185` (Rose) | Live indicator |
| `--sky` | `#0ea5e9` (Sky) | Loading, timers |
| `--bg` | `#08090e` | Page background (near-black) |
| `--surface` | `rgba(255,255,255,0.04)` | Glass card background |
| `--border` | `rgba(255,255,255,0.08)` | Borders, dividers |
| `--text` | `#e2e8f0` | Primary text |
| `--text-secondary` | `#94a3b8` | Secondary text |
| `--text-tertiary` | `#64748b` | Labels, hints |

> **Anti-AI-slop note**: Tailwind indigo (#6366f1) is banned as accent.
> Accent usage is capped at ~2 visible uses per screen.
> All icons use 1.7px-stroke monoline SVG with currentColor, never emoji.

### Typography
- **Display/Headlines**: Plus Jakarta Sans, 700-800 weight
- **Body/UI**: Plus Jakarta Sans, 400-600 weight
- **Monospace (Logs, Codes)**: Space Mono, 400-700 weight

### Spacing Grid
- Base unit: `8px`
- Standard gaps: 16px, 20px, 24px, 32px
- Padding: 12px (compact), 16px (standard), 24px (generous)

### Animations
- **Entrance**: `slideInUp` (0.5s ease) for cards
- **Hover**: `scale(1.05)` + shadow for buttons
- **Loading**: `spin` (0.8s linear) for spinner
- **Pulse**: `pulse` (2s infinite) for live indicator

---

## Component Library

### Buttons

#### Variants
```
Primary (Indigo)
├─ Default
├─ Hover: scale(1.05) + shadow
├─ Disabled: opacity 0.5
└─ Loading: spinner inside

Success (Green) – Correct answers
Danger (Red) – Wrong/eliminations
Neutral (Slate) – Skip, secondary
```

#### Button Styles
```css
.btn-large {
    padding: 16px 24px;
    font-size: 16px;
    font-weight: 700;
    border-radius: 8px;
}

.btn-sm {
    padding: 6px 10px;
    font-size: 12px;
}
```

### Status Badges
```
Round X | Question 1/2 | Live (pulsing)
├─ Background: rgba(accent, 0.15)
├─ Border: 1px accent
└─ Font: 12px bold, uppercase
```

### Player Row/Card
```
[Avatar] Name  ❤️ ❤️ ❤️  [+] [−] [Menu]
├─ Name: 14px bold
├─ Lives: hearts or count (e.g., "3")
├─ Controls: +/− life buttons (host only)
└─ State: normal / eliminated (opacity 0.5)
```

### Log Entry Row (Host-only)
```
[14:23:45] Lisa "Question text..." ✓ RICHTIG ❤️ -1
├─ Timestamp: Space Mono, 11px, sky-blue
├─ Player name: lime-green
├─ Result badge: green/red/gray (small pill)
└─ Optional life change indicator
```

### Vote Card
```
[Card: Player Name]
├─ Default: dark card, gray border
├─ Hover: blue border, blue tint
├─ Selected: blue background, white text, scale 1.02
└─ Disabled: opacity 0.3, cursor not-allowed
```

---

## Screen Designs

### HOST VIEW (Desktop, 1440px)

#### Header
```
[Title: "Der Dümmste fliegt"]  [Room: PARTY] [R2] [Q1/2] [LIVE◯]  [Links] [End]
```
- Gradient title (lime → sky)
- Status badges (room, round, question, live)
- Action buttons: Copy Host Link, Copy Player Link (OTP), End Game

#### Main Panel (Center)
1. **Player Card**: Large text, current target name
2. **Question Card**: 
   - Border: 2px lime
   - Background: gradient (lime 15% + sky 15%)
   - Font: 24px bold
3. **Answer Card**:
   - Border: 2px dashed green
   - Label: "✓ Korrekte Antwort (nur Host)"
   - Font: 24px bold
4. **Action Buttons** (3-column grid):
   - Richtig (green gradient)
   - Falsch (red gradient)
   - Skip (sky gradient)
   - On hover: scale 1.05 + shadow

#### Sidebar (Right, 380px)
**Top Panel: Player Dashboard**
- All players listed with hearts
- +/− buttons per player (host only)
- Eliminated state: opacity 0.5

**Bottom Panel: Live Log**
- Scrollable (max 400px)
- Most recent at top
- Font: Space Mono 11px
- Color-coded results (green/red/gray pills)
- Timestamp, player, question (short), result, life change

---

### PLAYER VIEW (Mobile, 390px)

#### Header
```
[Room Code: PARTY]
[Runde 2 • Frage 1/2]
```

#### Main Content
1. **Question Display**:
   - Large, centered text (20px bold)
   - Gradient border (lime → sky)
   - Hint: "💬 Antworte über Voice/Discord"
   - Min height: 200px (centered)

2. **Waiting State**:
   - Spinner + "Host bewertet gerade..."
   - Shown while host evaluates answer

3. **Player Dashboard**:
   - All players with lives (everyone sees all)
   - Self highlighted (lime background + border)
   - Eliminated: opacity 0.5, red border

#### Voting Screen
```
[Title: "🗳️ Wer war am dümmsten diese Runde?"]
[Rules box: "Du darfst 1× abstimmen. Nicht für dich selbst."]
[Cards: Lisa | Tom]  (self-name disabled/hidden)
[Submit] [Back]
```
- Vote cards: 1 selected = blue background, scale 1.02
- Self player: hidden or disabled (opacity 0.3)
- Prevent double-voting: show "Du hast bereits abgestimmt" state

#### Voting Confirmation
```
[Checkmark] ✓
"Deine Stimme wurde registriert!"
[Back to Game]
```

#### Elimination Screen
```
[Title: "💔 Du bist raus!"]
[Message: "0 Leben übrig. Schau dir den Rest an!"]
[Spectate Button]
```

#### Estimation Game Screen
```
[Title: "🎯 Schätzspiel!"]
[Message: "Das Spiel wechselt ins Schätzspiel-Modus."]
[OK Button]
```

---

### LOBBY VIEW (Desktop, 1440px, Pre-game)

#### Header
```
[Large Title: "Der Dümmste fliegt 🎮"]
[Subtitle: "Erstelle einen Room und lade Spieler ein"]
```
- Title: Gradient (lime → sky → coral)
- Animation: slideInDown

#### Content Grid (2 columns)

**Left Panel: Host Settings**
1. Host-Link (copyable)
2. Player-Link mit OTP (copyable)
3. Start Game button (large, lime gradient)

**Right Panel: Joined Players**
```
[Avatar: M] Max
[Avatar: L] Lisa
[Avatar: T] Tom
```
- Scrollable if many players
- Count badge: "🎮 Beigetretene Spieler (3)"

---

## Interaction Flows

### Host Control Flow
```
1. Lobby
   → Show Room Code + Links
   → Players join via OTP link (name pre-filled)
   → Host clicks "Start Game"

2. Game Screen
   → Host sees current player + question
   → Host clicks Correct/Wrong/Skip
   → If Skip: new question for same player
   → If Correct/Wrong: advance to next player
   → After all players answered (2 questions each): Voting Phase

3. Voting Phase
   → Each player votes (1× per round, no self-vote)
   → Host can see votes in dashboard
   → Host triggers next round or end phase

4. End Conditions
   → Only 2 players with >0 lives → Estimation Game screen
   → All but 1 eliminated → Game Over
```

### Player Flow
```
1. OTP Link Join
   → URL: ...?name=Max&otp=xyz789
   → Direct to Game (no additional name input)

2. Question Screen
   → Shows question only (no answer field)
   → Hint: "Answer via voice/Discord"
   → Waiting state: spinner + "Host bewertet..."

3. Voting (end of round)
   → Vote card selection (self disabled)
   → Submit vote
   → Confirmation state

4. Elimination Path
   → If 0 lives → Elimination screen
   → Option to spectate (read-only dashboard)

5. Estimation Game
   → Placeholder screen (details TBD)
   → Transition trigger: only 2 players with >0 lives
```

---

## Responsive Design

### Host View
- **Desktop (1440px)**: 2-column layout (main + sidebar 380px)
- **Tablet (1024px)**: Stack to 1 column, sidebar becomes 2-column grid

### Player View
- **Mobile (390px)**: Full-width stack (header → question → dashboard)
- **Landscape**: Consider horizontal scroll or resizing

---

## States & Edge Cases

### Host States
- **Lobby**: Waiting for players
- **Game – No players**: "Awaiting first question..."
- **Game – Active**: Show current player + question
- **Game – Voting phase**: Player dashboard + vote count
- **End Game Modal**: Confirm end game action

### Player States
- **Waiting for host decision**: Spinner + text
- **Voting**: Vote card selection + confirmation
- **Eliminated**: Dark/greyed out, spectate option
- **Estimation Game**: Placeholder screen

### Error States
- **OTP Expired**: "Link abgelaufen. Frag Host um neuen Link"
- **Room Not Found**: "Raum existiert nicht"
- **Name Already Taken**: "Name schon vergeben. Versuche einen anderen"
- **Connection Lost**: Reconnect button

---

## Assets & Exports (Figma)

### Named Frames
1. `HOST_DESKTOP_1440` – Host view (full screen)
2. `PLAYER_MOBILE_390` – Player view (full height)
3. `LOBBY_DESKTOP_1440` – Lobby screen
4. `COMPONENTS` – Component library page

### Component Instances
- `Btn/Primary`
- `Btn/Success`
- `Btn/Danger`
- `Btn/Neutral`
- `Badge/Status`
- `Badge/Result`
- `PlayerRow`
- `LogEntry`
- `VoteCard`
- `Card`

### Export Settings
- **Format**: PNG (for preview) + Design Specs
- **Scale**: 1x
- **Naming Convention**: `[Frame]_[Component].png`

---

## Implementation Notes

### HTML/CSS Structure
- **CSS Variables**: All colors, spacing, fonts use CSS variables for consistency
- **Flexbox + Grid**: Responsive layouts using modern CSS
- **Animations**: CSS-only (no dependencies) for performance
- **Dark Theme**: Primary design (light text on dark background)

### JavaScript Interactivity
- **View switching**: Click buttons to toggle host/player/lobby
- **Button interactions**: Feedback on Correct/Wrong/Skip clicks
- **Voting flow**: Select vote → submit → confirmation
- **Toasts**: Inline feedback messages (2s duration)
- **Responsive**: Media queries for tablet/landscape views

### Performance
- No external libraries (fonts only from Google Fonts)
- CSS-only animations (GPU-accelerated)
- Minimal DOM manipulation
- Fast viewport load time

---

## Next Steps for Development

1. **Connect to backend API**
   - POST `/game/answer` with result
   - POST `/game/vote` with selected player
   - WebSocket for real-time player updates

2. **Database schema**
   - Games, Rooms, Players, Questions, Answers, Votes
   - Session/OTP management

3. **Question database**
   - Store questions with categories, difficulty
   - Prevent duplicate questions per session
   - Support for multiple languages

4. **Live updates**
   - Host sees player votes in real-time
   - Player sees dashboard updates (eliminated players, round changes)
   - Voting phase trigger → all players see voting screen simultaneously

5. **Mobile optimization**
   - Landscape support
   - Touch-friendly buttons (min 44px)
   - Portrait lock for game screen

---

## Color & Branding Notes

The design uses a **dark, playful quiz-show vibe** with:
- **Dark gradient background** (slate → midnight) for immersive party feel
- **Lime & Sky accents** for energy and party energy
- **Gradient text** for title (lime → sky) suggests movement, excitement
- **Color-coded feedback**: Green (correct), Red (wrong), Gray (skip) → instant visual understanding
- **Pulsing live indicator** → real-time tension
- **Bold typography** (Space Mono + Jakarta Sans) → readable even on small screens, playful character

This is intentionally **not minimalist** – it's maximalist party energy while staying organized and readable.
