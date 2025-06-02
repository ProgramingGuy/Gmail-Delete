# Email Delete, GMail.
Mass email delete from sender, first right click the email and click find emails from and it'll display all emails from that sender. Then paste the code in the dev console and run it and it'll delete all the emails from that sender.

```javascript
(async function deleteAllEmailsAutomated() {
  const wait = ms => new Promise(res => setTimeout(res, ms));

  function simulateMouseEvent(element, eventType) {
    const event = new MouseEvent(eventType, {
      view: window,
      bubbles: true,
      cancelable: true,
      buttons: 1,
    });
    element.dispatchEvent(event);
  }

  while (true) {
    const emailRows = document.querySelectorAll('tr[role="row"]');
    if (!emailRows || emailRows.length === 0) break;

    const selectAllContainer = document.querySelector('div[aria-label="Select"]');
    if (!selectAllContainer) break;
    const checkbox = selectAllContainer.querySelector('span[role="checkbox"]');
    if (checkbox && checkbox.getAttribute('aria-checked') === 'false') checkbox.click();

    await wait(500);

    const firstEmailRow = document.querySelector('tr[role="row"]');
    if (!firstEmailRow) break;
    firstEmailRow.dispatchEvent(new MouseEvent('contextmenu', {
      bubbles: true,
      cancelable: true,
      view: window,
      button: 2,
      buttons: 2,
    }));

    await wait(500);

    const menuItems = [...document.querySelectorAll('[role="menuitem"]')];
    const deleteMenuItem = menuItems.find(item => item.textContent.trim().toLowerCase() === 'delete');
    if (!deleteMenuItem) break;

    // Simulate mouse events with shorter delays
    simulateMouseEvent(deleteMenuItem, 'mouseover');
    await wait(50);
    simulateMouseEvent(deleteMenuItem, 'mousedown');
    await wait(50);
    simulateMouseEvent(deleteMenuItem, 'mouseup');
    await wait(50);
    simulateMouseEvent(deleteMenuItem, 'click');

    await wait(3000); // wait for Gmail to process
  }
  console.log("Done deleting all emails.");
})();
```
