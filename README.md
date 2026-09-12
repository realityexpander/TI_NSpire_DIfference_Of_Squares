# TI-Nspire Non-CAS Universal Difference of Squares Factorer

An advanced TI-Basic custom function designed to automatically identify and factor any **Difference of Squares** binomial variation on a **Non-CAS TI-Nspire** (including the standard **CX** and **CX II** models).

Standard non-CAS calculators completely lack the symbolic algebra engine required to factor complex expressions or force linear terms like $x - 16$ into radical factors. This script solves that restriction by evaluating text strings dynamically inside a sandboxed environment, extracting mathematical properties purely through numerical tricks and base-2 logarithms.

## Core Capabilities

- **Radical Factorization for Linear Terms:** This program is specifically engineered to handle expressions where the variable is to a **power of one**, yet the user expects a square result. It successfully breaks down linear terms into radical factors (e.g., $x - 16 \rightarrow (\sqrt{x}-4)(\sqrt{x}+4)$), a feat natively impossible on non-CAS architectures.
- **Universal Exponent Detection:** Handles standard quadratics ($x^2$), higher even-powers ($x^4$, $x^8$), odd powers ($x^5$), and linear expressions ($x^1$).
- **Coefficient Aware:** Correctly tracks and factors leading numbers attached to variables (e.g., $4x^2 - 64 \rightarrow (2x - 8)(2x + 8)$).
- **Fractional Bracket Grouping:** Automatically places proper formatting parentheses around non-integer fractional powers (e.g., $x^{(5/2)}$).
- **Strict Architecture Compliance:** Built with exactly **one statement per line** and zero inline block colons (`:`), fulfilling strict TI-Basic compiler rules.

---

## Technical Magic: How it Works Without CAS

Since the native non-CAS environment on devices like the TI-Nspire CX II crashes if you try to query variables or perform substitution on undefined tokens like `x`, this program implements two unique workarounds:

1. **The Sandbox Variable Escape:** By listing `x` in the `Local` statement declaration, the program builds a private sandbox. This lets the script legally mutate the local `x` with sequential values (`x:=0`, `x:=1`, etc.) without throwing an *Undefined Variable* fault. The `expr()` function then safely processes the string segment-by-segment.
2. **Logarithmic Degree Recovery:** To extract the hidden polynomial power without an algebraic parsing engine, the math exploits the following relationship:
   
   $$\text{term\_x2} = A \cdot 2^d$$
   
   Dividing it by the isolated leading coefficient ($A$) isolates the base component: $2^d$. Applying a basic algebraic change-of-base rule extracts the polynomial degree flawlessly:
   
   $$d = \frac{\log(2^d)}{\log(2)}$$

---

## Installation Details

To add this function to your TI-Nspire CX or CX II (Non-CAS):

### Step 1: Create the Function
1. Press the **Home** key and open a standard **Calculator** document.
2. Press **Menu** > **9: Functions & Programs** > **1: Program Editor** > **1: New**.
3. Configure the properties box:
   - **Name:** `ds` (or any shortcut name you prefer)
   - **Type:** Change this from Program to **Function**.
   - **Access:** Set to **Public (Library)** if you want to use it across multiple files.
4. Click **OK**. A split-screen window containing the code editor will appear.

### Step 2: Input the Script
Paste or type the structured code between the `Func(string_in)` and `EndFunc` lines. Ensure each statement sits cleanly on its own line and that any tracking comments begin flush with the left margin using the `©` marker.

### Step 3: Check and Save
1. While your cursor is active inside the code editor side of the screen, press **Menu**.
2. Select **1: Check Syntax & Store** > **1: Check Syntax & Store** (or use the shortcut **Ctrl + B**).
3. Look at the top-left status bar—it must display *"ds stored successfully"*. If it displays a syntax error, verify that no two commands share a single line.
4. Press **Ctrl + K** to close the code editor and return to your full calculator workspace.

---

## How To Use

Because this program runs on a Non-CAS system, you **must wrap the target expression in quotation marks** so it is parsed cleanly as a string argument.

### Examples

| Input Syntax | Calculator Output | Output Description |
| :--- | :--- | :--- |
| `ds("x - 16")` | `"(√(x) - 4)*(√(x) + 4)"` | **Linear power down into radical factors (Square Result)** |
| `ds("4*x^2 - 64")` | `"(2*x - 8)*(2*x + 8)"` | Successfully extracts and splits leading coefficients |
| `ds("x^4 - 256")` | `"(x^2 - 16)*(x^2 + 16)"` | Scales to higher even-order differences |
| `ds("x^5 - 4096")` | `"(x^(5/2) - 64)*(x^(5/2) + 64)"` | Dynamically applies parentheses to odd fractional powers |
