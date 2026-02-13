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

## Exercise 1: XR Interaction Toolkit Basics  
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

---

## Exercise 2: Build an XR Interaction Toolkit sample scene to Meta Quest (Android)

You’ll take one of the **XR Interaction Toolkit** sample scenes (**Starter Assets** or **Hands Demo**) and deploy it to a **Meta Quest** headset as an **Android** build using **OpenXR**.

## Goal

- Switch the project to the **Meta Quest / Android** build target
- Enable **OpenXR** with **Meta Quest** features + the right interaction profile
- **Build & Run** on-device (Quest)

> Meta’s current recommended path for Quest in Unity is **OpenXR** with the **Meta Quest feature group**.

---

## 1) Prereqs (one-time)

### A) Unity install modules

In **Unity Hub**, ensure your **Unity 6.3+** editor install includes:

- Android Build Support  
- OpenJDK  
- Android SDK & NDK tools  

> These are required to produce APKs/AABs.

### B) Headset setup

On your **Quest**:

- Enable **Developer Mode**
- Enable **USB debugging**
- Connect via **USB-C** and accept the debugging prompt in-headset

---

## 2) Choose a sample scene to build

Pick one:

**Controller:**
- `Samples/XR Interaction Toolkit/3.3.1/Starter Assets/DemoScene.unity`

**Hands:**
- `Samples/XR Interaction Toolkit/3.3.1/Hands Interaction Demo/HandsDemoScene.unity`

Open the scene, then:

- **File → Build Profiles / Build Settings → Add Open Scenes**  
  (so the scene is included in the build)

---

## 3) Switch to the Meta Quest / Android build target

In Unity:

- **File → Build Profiles**
- Select **Meta Quest** platform if available (Unity 6.x), then **Enable / Switch Platform**  
  - If you don’t see **Meta Quest**, switch to **Android**.

---

## 4) Enable OpenXR for Quest

Go to:

- **Edit → Project Settings → XR Plug-in Management**

### A) Android tab: enable OpenXR + Meta Quest feature group

- Under **Plug-in Providers (Android)**: enable **OpenXR**
- Enable the **Meta Quest feature group** (wording varies slightly by package/version)

### B) OpenXR settings: verify Meta Quest Support features

Still in **Project Settings → XR Plug-in Management → OpenXR**, ensure **Meta Quest support** features are enabled/configured (Meta’s docs call this out under “Meta Quest Support”).

### C) Interaction profile (controllers)

In the **OpenXR (Android)** section, add/enable:

- **Oculus Touch Controller Profile**

> If you’re building the **Hands Demo**, keep your **XR Hands + Hands Interaction Demo** setup as-is; **OpenXR still needs to be enabled** for Quest builds.

---

## 5) Player / Android build settings

Go to:

- **Edit → Project Settings → Player**

Common “must not forget” items for Quest Android builds:

- **Active Input Handling:** keep consistent with your project (many XR setups use the **Input System**)
- **Scripting Backend:** **IL2CPP**
- **Target Architectures:** **ARM64**

> Exact labels vary by Unity version, but these are the typical Quest requirements.

---

## 6) Build & Run on the headset

1. Connect Quest via USB (**USB debugging accepted**).
2. Open **File → Build Profiles / Build Settings**
3. Confirm your sample scene is in **Scenes In Build**
4. Click **Build And Run**
5. Choose an output folder (Unity will generate an APK and install it)

If it installs successfully, the app should appear in the headset under:

- **Apps → Unknown Sources** (location can vary by OS version)

---

## Quick check

Launch the app on the Quest and verify:

- Head tracking works
- Controllers appear (**Starter Assets**) *or* hands appear (**Hands Demo**)
- You can interact with objects/UI in the scene

