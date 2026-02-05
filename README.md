# Virtual Reality Demo Project (ABA 2026)

A Unity XR demo project showcasing **controller-based** and **hand-tracking** interactions using Unity’s XR tooling + sample scenes.

---

## Requirements

- **Unity 6.3+ LTS** (URP project)
- Installed packages:
  - **XR Interaction Toolkit**
    - Samples: *Starter Assets*, *XR Interaction Simulator*, *Hands Interaction Demo*
  - **XR Hands**

---

## Getting Started

1. Download the repository (ZIP) or clone it.
2. Unzip (if needed).
3. Open **Unity Hub** → **Add** → select the project folder.
4. Open the project in **Unity 6.3+ LTS**.

---

## Demo Scenes

### Controller-based interactions

Open:

- `Samples/XR Interaction Toolkit/3.3.1/Starter Assets/DemoScene.unity`

### Hands-based interactions

Open:

- `Samples/XR Interaction Toolkit/3.3.1/Hands Interaction Demo/HandsDemoScene.unity`

---

## Running Without a VR Headset (XR Interaction Simulator)

Enable the simulator:

1. **Edit** → **Project Settings** → **XR Plug-in Management** → **XR Interaction Toolkit**
2. Tick: **Use XR Interaction Simulator in scenes**

---

## Running With a VR Headset

Disable the simulator:

1. **Edit** → **Project Settings** → **XR Plug-in Management** → **XR Interaction Toolkit**
2. Untick: **Use XR Interaction Simulator in scenes**

---

## Exercise: XR Interaction Toolkit Basics  
### XR Button toggles a TextMeshPro label (World Space UI)

Open the project, start a demo scene, and experiment with XR interactions. Then add a simple XR UI button that toggles a TextMeshPro label when pressed.

---

## Goal

- Add an **XR UI Canvas** (World Space)
- Add a **Button (TextMeshPro)**
- Create a script with a **ToggleText()** function
- Wire the button **OnClick** event to call the toggle function

---

## 1) Add an XR UI Canvas (World Space)

1. In the **Hierarchy**:  
   `Right Click → XR → UI Canvas`

2. Select the **Canvas** and set:

   **Canvas**
   - **Render Mode:** `World Space`

   **Rect Transform**
   - **Scale:** `0.01, 0.01, 0.01`
   - **Position:** `0, 1, -2.5`
   - **Width / Height:** `100 / 50`

> 💡 If the UI appears enormous or tiny, the **Canvas scale** is usually the issue. `0.01` is a common starting point for world-space UI.

---

## 2) Add a TextMeshPro Button

1. Under your XR UI Canvas in the **Hierarchy**:  
   `Right Click → UI → Button - TextMeshPro`

2. Select the **Button** and set:

   **Rect Transform**
   - **Width / Height:** `100 / 50`

> If Unity prompts you, import **TextMeshPro Essentials**.

---

## 3) Create the script (TextSwitcher.cs)

1. Create a new C# script named `TextSwitcher.cs`.
2. Create an empty GameObject named `TextManager`.
3. Attach `TextSwitcher.cs` to `TextManager`.

Paste this code into `TextSwitcher.cs`:

```csharp
using TMPro;
using UnityEngine;

public class TextSwitcher : MonoBehaviour
{
    [Header("Text to change")]
    [SerializeField]
    private TextMeshProUGUI targetText;

    [Header("Toggle values")]
    [SerializeField]
    private string textA = "Hello!";

    [SerializeField]
    private string textB = "Goodbye!";

    private bool showingA = true;

    public void ToggleText()
    {
        if (!targetText)
            return;

        targetText.text = showingA ? textB : textA;
        showingA = !showingA;
    }
}
```

## 4) Wire the Button to the script

### A) Assign the Target Text reference

1. Select `TextManager` (the object with `TextSwitcher` attached).
2. In the Inspector, find the `TextSwitcher` component.
3. Expand the Button in the Hierarchy and drag the child **Text (TMP)** into **Target Text**.

✅ Make sure you drag the **Text (TMP)** child object, **not** the Button root.

### B) Connect the Button click event

1. Select the **Button** object.
2. In the **Button** component, under **On Click ()**:
   - Click the **+** button to add a new event.
   - Drag the `TextManager` object into the object field.
   - From the function dropdown, choose:  
     `TextSwitcher → ToggleText()`

---

## Quick check

- Enter Play Mode.
- Press the button in XR.
- The label should toggle between **Hello!** and **Goodbye!**.
