# 🚦 PLC Mode & Function Cycling Simulator

This project simulates a PLC-based control system that continuously cycles through two operating **Modes** — `Mode-1` and `Mode-2` — each executing two **Functions** (`F1` and `F2`) in timed intervals. It was developed using **Allen-Bradley RSLogix 500**, tested with **RSLogix Emulator**, and monitored via **RSLinx Classic Lite**.

The logic updates memory to reflect the current state and counts how many times each state is entered.

---

## 🛠️ Tools Used

- 🎛️ **RSLogix 500** – Allen-Bradley's PLC programming environment  
- 🧪 **RSLogix Emulator** – Simulates controller behavior without physical hardware  
- 🧩 **RSLinx Classic Lite** – Used to view real-time data during simulation  

---

## 🔄 System Behavior

The program cycles continuously through the following sequence:
Mode-1 → F1 → F2 → Mode-2 → F1 → F2 → (repeat)
Each state runs for 10 seconds.


Each function lasts for **10 seconds**, and on every transition:
- The **mode** and **function names** are stored as strings.
- A combined string (mode + function) is created.
- A cycle counter is incremented depending on the current state.

---

## 📊 Memory Map

| Address | Purpose                          | Example Value     |
|---------|----------------------------------|-------------------|
| `ST9:0` | Current Mode                     | `"Mode-1"`        |
| `ST9:1` | Current Function                 | `"F2"`            |
| `ST9:2` | Combined Mode + Function         | `"Mode-2F1"`      |
| `N7:0`  | Mode-1 / F1 Cycle Counter        | `5`               |
| `N7:1`  | Mode-1 / F2 Cycle Counter        | `5`               |
| `N7:2`  | Mode-2 / F1 Cycle Counter        | `5`               |
| `N7:3`  | Mode-2 / F2 Cycle Counter        | `5`               |

---

## ✅ Test Criteria

Once the program is loaded and running in the emulator:

- `ST9:0`–`ST9:2` update every 10 seconds with the current mode, function, and combined string.
- Counters in `N7:0`–`N7:3` increment in the expected order:
(0, 0, 0, 0)
(1, 0, 0, 0)
(1, 1, 0, 0)
(1, 1, 1, 0)
(1, 1, 1, 1)
(2, 1, 1, 1)
Each number increases only when the corresponding state is active.


---

## 🧠 Logic Overview

- **Timers (TON)** control the 10-second delay between state transitions.
- **MOV instructions** update the string values in `ST9:0`, `ST9:1`, and `ST9:2`.
- The state machine cycles in this order:  
  `M1F1 → M1F2 → M2F1 → M2F2 → repeat`.
- **ADD instructions** are used to manually increment the cycle counters (`N7:0` to `N7:3`), instead of standard `CTU` counters. Each counter increases when its specific state is active.

---

## 📸 Visual Overview

### Mode/Function Cycle Diagram  
![Cycle Diagram](docs/mode-function-diagram.png)

### RSLogix Emulator in Action  
![Emulator Screenshot](docs/emulator-screenshot.png)

---

## 🙋 Author

**Sankeerth Pradeep**  
🔗 [[https://github.com/SankeerthPradeep](https://github.com/SankeerthPradeep)]

---

## 📄 License

This project is licensed under the **MIT License**.  
You are free to use, modify, and distribute this project.

---

## ⭐ Tips for Use

- Open `PLC_Cycle.ACD` in RSLogix 5000 or Studio 5000.
- Load the project into RSLogix Emulator.
- Use RSLinx Classic Lite to monitor `ST9:` and `N7:` values.

  
