# pad-toggle-fix

Power Automate Desktop flow that sets a web toggle to a desired state when the normal UI click fails.

## Evidence (one run, In_ToggleSelector = #tC, In_DesiredState = on)
| Step | State |
|---|---|
| Before | C=off |
| After UI click (Press button) | C=off - click ignored |
| After Run JavaScript function | C=on - result ON |

Screenshots: `evidence/01-js-fix-success.png`, `evidence/02-config-invalid.png`

## Design
- Frame: Init_Context > Init_Application > Execute > Finalize > Terminate
- Inputs validated up front: ConfigMissing (no selector), ConfigInvalid (state not on/off)
- JavaScript tries click(), then a pointerdown event; returns ON, OFF, ALREADY_ON, ALREADY_OFF, NOT_FOUND, IGNORED or JS_ERROR
- Result checked against an allowlist; anything else throws ToggleFailed
- Errors caught in Execute, browser still closed in Finalize, logged in Terminate

## Inputs
| Name | Example | Rule |
|---|---|---|
| In_LabUrl | file:///C:/Training/toggle-lab.html | Page to open |
| In_ToggleSelector | #tC | CSS selector, required |
| In_DesiredState | on | on or off only |

## Known limit
The selector is placed inside a single-quoted JS string, so selectors containing ' are not supported. Use id selectors.

## Files
- flow/ - exported subflows (copy of each subflow's actions as text)
- lab/toggle-lab.html - test page with three toggle types
- evidence/ - run screenshots
