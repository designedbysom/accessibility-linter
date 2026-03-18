# Example Scenarios

Real-world examples of how accessibility-linter works during the design and build process. Each scenario shows the component being built, what gets baked in automatically (Tier 1), and what gets raised as a question before building (Tier 2).

---

## 1. E-commerce product card

**What you're building:** A card with a product image, title, price, and "Add to cart" button. The whole card is clickable and links to the product detail page.

**Baked in (Tier 1):**
- `alt` text on the product image describing the product, not the image format
- `<a>` wrapping the card for navigation, not a `<div onClick>`
- "Add to cart" button uses `<button>` with descriptive label like "Add [Product Name] to cart", not just "Add"

**Raised before building (Tier 2):**
If the whole card is clickable AND the button inside is also interactive, these are two overlapping click targets. The question: "The card links to the product page, and the button adds to cart. These are two different actions on overlapping surfaces. Should the card link wrap everything except the button, or should only the title be the link? Nesting interactive elements inside each other breaks keyboard and screen reader expectations."

---

## 2. Dashboard filter sidebar

**What you're building:** A sidebar that opens when you click "Filters" on a data dashboard. Contains dropdowns, date pickers, and a checkbox group for status types.

**Baked in (Tier 1):**
- `<fieldset>` + `<legend>` around the checkbox group for status types
- `<label>` on every input, including date pickers
- `aria-expanded="true/false"` on the Filters trigger button

**Raised before building (Tier 2):**
"Does this filter sidebar open as a modal (focus trap, user must close it to continue) or as a panel alongside the dashboard (no focus trap, user can interact with both)? If it's a panel, focus should move to the first filter on open but not be trapped. If it's a modal, `Escape` closes it and focus returns to the Filters button. These are different implementations."

---

## 3. Toast notification system

**What you're building:** A notification system that shows success/error/info toasts in the bottom-right corner. Toasts auto-dismiss after 5 seconds.

**Baked in (Tier 1):**
- `role="status"` with `aria-live="polite"` on the toast container for non-critical messages
- `role="alert"` for error toasts
- Color + icon for each toast type, not color alone
- Pause auto-dismiss on hover and focus

**Raised before building (Tier 2):**
"If multiple toasts can stack, how should they be announced? Announcing every toast in rapid succession overwhelms screen reader users. Consider: should only the most recent toast be announced, or should they queue? Also, 5 seconds may not be enough time for screen reader users to read the content. Should the timer reset when the toast receives focus?"

---

## 4. Multi-step checkout form

**What you're building:** A three-step checkout flow: Shipping, Payment, Review. Each step is in a modal overlay. User can go back to previous steps.

**Baked in (Tier 1):**
- `aria-invalid="true"` and error messages linked via `aria-describedby` on invalid fields
- `<label>` on every input, including card number, expiry, CVV
- Step indicator uses `aria-current="step"` on the active step
- `aria-busy="true"` on the form while payment is processing

**Raised before building (Tier 2):**
"This is a multi-step form inside a modal where accidental dismissal means the user loses their shipping and payment data. I'd disable `Escape` to close and click-outside to close, and add an explicit 'Cancel order' button with a confirmation dialog if any fields have been filled. Does that match the intent?"

---

## 5. Navigation dropdown menu

**What you're building:** A top navigation bar with dropdown menus. Hovering over "Products" reveals a mega-menu with categories and featured items.

**Baked in (Tier 1):**
- `aria-expanded="true/false"` on the trigger button
- `aria-haspopup="true"` on triggers with dropdowns
- Menu items are a `<ul>` with `<li>`, not styled `<div>`s

**Raised before building (Tier 2):**
"This menu opens on hover, but hover doesn't exist for keyboard or touch users. The trigger needs to work on click/`Enter`/`Space` as well. For keyboard navigation: should arrow keys move between menu items (standard menu pattern), or should `Tab` move through them (simpler, but non-standard for menus)? The standard pattern is `Enter`/`Space` to open, arrow keys to navigate items, `Escape` to close and return focus to the trigger."

---

## 6. Data table with sortable columns

**What you're building:** A table showing user data (name, email, role, last active). Column headers are clickable to sort ascending/descending.

**Baked in (Tier 1):**
- `<table>`, `<thead>`, `<tbody>`, `<th>`, `<td>` (not `<div>` grid layout)
- `aria-sort="ascending"`, `aria-sort="descending"`, or `aria-sort="none"` on each sortable `<th>`
- Sort buttons inside `<th>` are actual `<button>` elements
- Sort icon + text direction indicator, not icon alone

**Raised before building (Tier 2):**
"When a column is sorted, should the change be announced to screen reader users? If the table is large and sorting triggers a visible reorder, a brief announcement like 'Sorted by name, ascending' via a live region would confirm the action happened. Without it, a screen reader user clicks sort and gets no feedback."

---

## 7. Search with autocomplete

**What you're building:** A search input that shows suggestions as you type. Selecting a suggestion navigates to that page.

**Baked in (Tier 1):**
- `role="combobox"` on the input
- `aria-expanded="true/false"` based on whether suggestions are visible
- `aria-controls` pointing to the suggestions listbox
- `aria-activedescendant` updating as the user arrows through suggestions
- `role="listbox"` on the suggestions container, `role="option"` on each suggestion

**Raised before building (Tier 2):**
"Two things to decide before building. First: when the user arrows to a suggestion and presses `Enter`, does it select the suggestion and submit the search, or just fill the input? These are different interaction models (select-and-go vs. select-then-submit). Second: how many suggestions trigger the list? If the list can show 20+ results, keyboard users need to be able to skip past it. Consider capping visible suggestions or adding a shortcut to exit the list."

---

## 8. Settings page with toggle switches

**What you're building:** An account settings page with toggles for email notifications, dark mode, two-factor auth, and marketing emails.

**Baked in (Tier 1):**
- Each toggle is a `<button>` with `aria-pressed="true/false"`, not a styled checkbox or `<div>`
- Descriptive labels: "Enable email notifications", not just "Notifications"
- `<fieldset>` + `<legend>` if toggles are grouped by category (e.g., "Notification preferences")
- Visible focus indicator on each toggle

**Raised before building (Tier 2):**
"The two-factor authentication toggle has higher stakes than the others. Disabling it affects account security. Should toggling it off trigger a confirmation dialog instead of an instant toggle? If so, that dialog needs its own focus management and should explain the security implications before confirming."

---

## 9. Image carousel

**What you're building:** A hero carousel on a landing page. Five slides with background images, headline text, and a CTA button. Auto-advances every 6 seconds.

**Baked in (Tier 1):**
- `alt` text on each slide image describing its content in context
- `prefers-reduced-motion` query to disable auto-advance and slide transitions
- Pause button for auto-advance, or auto-advance stops entirely on any user interaction
- Current slide indicator uses `aria-current="true"` on the active dot

**Raised before building (Tier 2):**
"How should keyboard users navigate between slides? Two common patterns: previous/next buttons flanking the carousel (simpler, each slide has a `Tab` stop for its CTA), or the dot indicators are focusable and arrow keys move between slides (more compact, but less discoverable). Which model fits this layout?"

---

## 10. Confirmation dialog for destructive action

**What you're building:** A dialog that appears when a user clicks "Delete project". Shows the project name and a warning that this can't be undone. Two buttons: Cancel and Delete.

**Baked in (Tier 1):**
- `role="alertdialog"` (not `role="dialog"`) because this requires immediate attention
- `aria-labelledby` pointing to the dialog title
- `aria-describedby` pointing to the warning message
- Focus moves to the dialog on open
- Focus trap inside the dialog
- `Escape` closes and cancels (safe default for destructive actions)
- Focus returns to the "Delete project" button on close

**Raised before building (Tier 2):**
"Which button should receive focus when the dialog opens? For destructive actions, the safe default is to focus the Cancel button, not Delete. This protects against accidental confirmation if the user is pressing `Enter` rapidly. Should the Delete button also require typing the project name to enable it, or is a single click enough?"

---

## 11. Drag-and-drop kanban board

**What you're building:** A project board with columns (To Do, In Progress, Done). Cards can be dragged between columns. Each card shows a task title, assignee, and priority tag.

**Baked in (Tier 1):**
- Each column uses a descriptive heading: `<h2>To Do</h2>`, not just a styled label
- Priority tags use text + color, not color alone (e.g., "High priority" with a red tag, not just a red dot)
- Cards are focusable with visible focus indicators

**Raised before building (Tier 2):**
"Drag-and-drop is inaccessible to keyboard and screen reader users by default. There needs to be a keyboard alternative. Common approach: focus a card, press a key (like `Space` or `Enter`) to pick it up, use arrow keys to move it between columns, press again to drop. Each move should be announced via a live region: 'Moved [task name] to In Progress.' Without this, keyboard users can see the board but can't interact with it. Should I build the keyboard move pattern into this, or is drag-and-drop an enhancement with a simpler alternative like a 'Move to' dropdown on each card?"

---

## 12. Inline edit on a profile page

**What you're building:** A user profile page where the display name and bio fields can be edited inline. Clicking an edit icon turns the text into an input field. Save and Cancel buttons appear.

**Baked in (Tier 1):**
- Edit button uses `<button aria-label="Edit display name">`, not an unlabelled icon
- When the field switches to edit mode, `<label>` is present on the input (even if visually hidden)
- Save and Cancel are `<button>` elements with descriptive text
- `aria-live="polite"` region to announce "Display name updated" on save

**Raised before building (Tier 2):**
"When the user clicks Edit, focus should move to the input field. When they click Save or Cancel, where should focus go? Back to the edit button is the standard expectation, but if Save triggers a success message, should focus move to the message first? Also: if the user presses `Escape` while editing, should it cancel the edit (discard changes) or do nothing? If it cancels, that's a data-loss path that should match Cancel button behavior."
