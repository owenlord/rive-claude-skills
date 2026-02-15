---
name: rive-embed
description: Help with embedding Rive animations in web and React applications. Provides code examples, API references, and troubleshooting for Rive runtime integration.
---

# Rive Embedding Guide

This skill helps you embed Rive animations in web and React applications.

## When to Use This Skill

Use this skill when you need to:
- Embed a Rive animation in a web page or React app
- Understand Rive runtime APIs and methods
- Troubleshoot embedding issues
- Configure animation playback, state machines, or inputs
- Optimize Rive performance in production

## Core Concepts

### Rive Runtime
The Rive runtime loads `.riv` files and renders animations in the browser using Canvas or WebGL.

**Key components:**
- **Rive instance**: Main controller for the animation
- **Canvas element**: Where the animation renders
- **Artboard**: The canvas/stage in your Rive file
- **State Machine**: Interactive animation logic
- **Inputs**: Variables you can control (booleans, numbers, triggers)

## Web Embedding (Vanilla JS)

### Basic Setup

```html
<!DOCTYPE html>
<html>
<head>
    <style>
        canvas {
            width: 500px;
            height: 500px;
        }
    </style>
</head>
<body>
    <canvas id="canvas"></canvas>
    
    <script src="https://unpkg.com/@rive-app/canvas@2.x"></script>
    <script>
        const r = new rive.Rive({
            src: 'your-animation.riv',
            canvas: document.getElementById('canvas'),
            autoplay: true,
            stateMachines: 'State Machine 1', // Optional: name of state machine
            onLoad: () => {
                console.log('Animation loaded');
                r.resizeDrawingSurfaceToCanvas();
            }
        });
    </script>
</body>
</html>
```

### NPM Installation

```bash
npm install @rive-app/canvas
```

```javascript
import { Rive } from '@rive-app/canvas';

const r = new Rive({
    src: '/path/to/animation.riv',
    canvas: document.getElementById('canvas'),
    autoplay: true,
    stateMachines: 'State Machine 1'
});
```

### Configuration Options

```javascript
const r = new rive.Rive({
    src: 'animation.riv',              // URL or path to .riv file
    buffer: arrayBuffer,                // Or use ArrayBuffer instead of src
    canvas: canvasElement,              // Canvas DOM element
    autoplay: true,                     // Start playing immediately
    stateMachines: 'State Machine 1',   // State machine name (string or array)
    animations: 'Animation 1',          // Timeline animation name (string or array)
    artboard: 'Artboard Name',          // Specific artboard (optional)
    layout: new rive.Layout({           // Layout configuration
        fit: rive.Fit.Cover,            // Cover, Contain, Fill, FitWidth, FitHeight, None, ScaleDown
        alignment: rive.Alignment.Center
    }),
    onLoad: () => {},                   // Callback when animation loads
    onLoadError: (error) => {},         // Callback on load error
    onPlay: () => {},                   // Callback when animation starts
    onPause: () => {},                  // Callback when animation pauses
    onStop: () => {},                   // Callback when animation stops
    onLoop: (event) => {},              // Callback on animation loop
    onStateChange: (event) => {}        // Callback on state machine state change
});
```

### Controlling Playback

```javascript
// Play/pause
r.play();
r.pause();
r.stop();
r.reset();

// Control state machine inputs
const inputs = r.stateMachineInputs('State Machine 1');
const trigger = inputs.find(i => i.name === 'Trigger Name');
trigger.fire();

const boolean = inputs.find(i => i.name === 'Boolean Name');
boolean.value = true;

const number = inputs.find(i => i.name === 'Number Name');
number.value = 42;

// Get current state
const stateNames = r.stateMachineNames; // Array of state machine names
```

### Layout and Sizing

```javascript
const r = new rive.Rive({
    src: 'animation.riv',
    canvas: canvas,
    layout: new rive.Layout({
        fit: rive.Fit.Cover,        // How animation fits in canvas
        alignment: rive.Alignment.Center,
        minX: 0,
        minY: 0,
        maxX: canvas.width,
        maxY: canvas.height
    }),
    onLoad: () => {
        // Resize canvas to match animation
        r.resizeDrawingSurfaceToCanvas();
    }
});

// Available Fit modes:
// - Fit.Cover: Fill canvas, may crop
// - Fit.Contain: Fit within canvas, may letterbox
// - Fit.Fill: Stretch to fill canvas
// - Fit.FitWidth: Fit to width
// - Fit.FitHeight: Fit to height
// - Fit.None: Original size
// - Fit.ScaleDown: Contain but never scale up
```

### Cleanup

```javascript
// Always cleanup when done
r.cleanup();
```

## React Embedding

### Using @rive-app/react-canvas

```bash
npm install @rive-app/react-canvas
```

### Basic React Component

```jsx
import { useRive } from '@rive-app/react-canvas';

function RiveAnimation() {
    const { RiveComponent } = useRive({
        src: 'animation.riv',
        stateMachines: 'State Machine 1',
        autoplay: true
    });

    return <RiveComponent />;
}
```

### Full React Example with Controls

```jsx
import { useRive, useStateMachineInput } from '@rive-app/react-canvas';

function InteractiveRive() {
    const { rive, RiveComponent } = useRive({
        src: 'animation.riv',
        stateMachines: 'State Machine 1',
        autoplay: true,
        onLoad: () => {
            console.log('Loaded');
        }
    });

    // Get input from state machine
    const hoverInput = useStateMachineInput(
        rive,
        'State Machine 1',
        'Hover',
        false // initial value
    );

    return (
        <div>
            <RiveComponent 
                style={{ width: 500, height: 500 }}
                onMouseEnter={() => hoverInput && (hoverInput.value = true)}
                onMouseLeave={() => hoverInput && (hoverInput.value = false)}
            />
            <button onClick={() => rive?.play()}>Play</button>
            <button onClick={() => rive?.pause()}>Pause</button>
        </div>
    );
}
```

### React Hook Options

```jsx
const { rive, RiveComponent } = useRive({
    src: 'animation.riv',
    buffer: arrayBuffer,                 // Alternative to src
    artboard: 'Artboard Name',
    stateMachines: 'State Machine 1',    // String or array
    animations: 'Animation 1',            // String or array
    autoplay: true,
    layout: new Layout({
        fit: Fit.Cover,
        alignment: Alignment.Center
    }),
    onLoad: () => {},
    onLoadError: () => {},
    onPlay: () => {},
    onPause: () => {},
    onStop: () => {},
    onLoop: () => {},
    onStateChange: () => {}
});

// rive object methods:
rive.play();
rive.pause();
rive.stop();
rive.reset();
rive.cleanup();
```

### Getting State Machine Inputs in React

```jsx
import { useRive, useStateMachineInput } from '@rive-app/react-canvas';

function App() {
    const { rive, RiveComponent } = useRive({
        src: 'animation.riv',
        stateMachines: 'State Machine 1',
        autoplay: true
    });

    // Boolean input
    const isHovering = useStateMachineInput(
        rive,
        'State Machine 1',
        'isHover'
    );

    // Number input
    const level = useStateMachineInput(
        rive,
        'State Machine 1',
        'Level'
    );

    return (
        <div>
            <RiveComponent />
            <button onClick={() => isHovering && (isHovering.value = !isHovering.value)}>
                Toggle Hover
            </button>
            <input 
                type="range" 
                min="0" 
                max="100"
                onChange={(e) => level && (level.value = Number(e.target.value))}
            />
        </div>
    );
}
```

### Firing Triggers in React

```jsx
const triggerInput = useStateMachineInput(
    rive,
    'State Machine 1',
    'TriggerName'
);

// Fire the trigger
<button onClick={() => triggerInput?.fire()}>
    Fire Trigger
</button>
```

## Common Patterns

### Loading from URL

```javascript
// Web
const r = new rive.Rive({
    src: 'https://cdn.rive.app/animations/vehicles.riv',
    canvas: canvas,
    autoplay: true
});

// React
const { RiveComponent } = useRive({
    src: 'https://cdn.rive.app/animations/vehicles.riv',
    autoplay: true
});
```

### Loading from File Upload

```javascript
// Web
const fileInput = document.getElementById('file-input');
fileInput.addEventListener('change', (e) => {
    const file = e.target.files[0];
    const reader = new FileReader();
    
    reader.onload = (event) => {
        const r = new rive.Rive({
            buffer: event.target.result,
            canvas: canvas,
            autoplay: true
        });
    };
    
    reader.readAsArrayBuffer(file);
});
```

### Responsive Canvas

```javascript
// Web - resize canvas when window resizes
window.addEventListener('resize', () => {
    r.resizeDrawingSurfaceToCanvas();
});

// React - with resize observer
import { useRive } from '@rive-app/react-canvas';
import { useEffect, useRef } from 'react';

function ResponsiveRive() {
    const containerRef = useRef(null);
    const { rive, RiveComponent } = useRive({
        src: 'animation.riv',
        autoplay: true
    });

    useEffect(() => {
        if (!rive || !containerRef.current) return;

        const observer = new ResizeObserver(() => {
            rive.resizeDrawingSurfaceToCanvas();
        });

        observer.observe(containerRef.current);
        return () => observer.disconnect();
    }, [rive]);

    return (
        <div ref={containerRef} style={{ width: '100%', height: '100vh' }}>
            <RiveComponent style={{ width: '100%', height: '100%' }} />
        </div>
    );
}
```

### Multiple Instances

```javascript
// You can have multiple Rive instances on the same page
const animation1 = new rive.Rive({
    src: 'animation1.riv',
    canvas: document.getElementById('canvas1'),
    autoplay: true
});

const animation2 = new rive.Rive({
    src: 'animation2.riv',
    canvas: document.getElementById('canvas2'),
    autoplay: true
});

// Remember to cleanup both
animation1.cleanup();
animation2.cleanup();
```

## Troubleshooting

### Animation not showing
1. Check that the canvas element exists before initializing Rive
2. Ensure canvas has width/height (CSS or attributes)
3. Check browser console for errors
4. Verify .riv file path is correct
5. Call `resizeDrawingSurfaceToCanvas()` after load

### State machine inputs not working
1. Verify state machine name matches exactly (case-sensitive)
2. Check input name matches exactly
3. Ensure animation has loaded before accessing inputs
4. Use `onLoad` callback to safely access inputs

### Performance issues
1. Use `Layout.fit = Fit.Cover` or `Fit.Contain` instead of `Fill`
2. Limit number of simultaneous Rive instances
3. Call `cleanup()` when component unmounts
4. Consider using lower resolution artboards for mobile

### React hooks returning undefined
1. Wait for `rive` to be defined before accessing inputs
2. Use optional chaining: `input?.value`
3. Check that component hasn't unmounted

## Best Practices

1. **Always cleanup**: Call `rive.cleanup()` when done (or when React component unmounts)
2. **Use state machines**: More powerful than timeline animations for interactive content
3. **Optimize artboard size**: Design at the size you'll display
4. **Preload animations**: Load Rive files early for better UX
5. **Handle loading states**: Show loading UI while .riv file loads
6. **Use Layout settings**: Control how animation scales and aligns
7. **Test on target devices**: Performance varies across devices

## Additional Resources

For the latest documentation, visit:
- Official docs: https://rive.app/community/doc/web-js/docvS86uz10s
- React canvas docs: https://github.com/rive-app/rive-react
- API reference: https://rive.app/community/doc/web-js/docvS86uz10s

When documentation is needed but might have changed, use web search to verify current APIs and examples.
