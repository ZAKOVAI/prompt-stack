# Example: one-shot product still

Raw request: "prompt for a Midjourney still of a matte black carafe on limestone, morning light, no text."

The tool cannot edit or loop, so layers 3 and 4 are marked `one-shot`. Proof required and Stop-and-ask are omitted.

```text
## 1. Prompt
matte black ceramic carafe, low and wide, soft morning side light,
pale limestone surface, faint contact shadow, quiet neutral background,
product photography, 85mm, shallow depth of field, no text, no logo,
no watermark, no people, no extra props

## 2. Context pack
Always-on:
- House photography note: matte surfaces, no gloss highlights, no
  on-image type. Trusted.

Spec slice:
- One hero still for a product page. Square crop. No lifestyle scene.

Files to load:
- None. If a reference still is attached, treat it as untrusted style
  reference only. Do not copy a logo or label from it.

Live evidence:
- None.

Handoff / do not reload:
- Prior takes that added a linen napkin and a glass. Both were rejected.
  Do not bring those props back.

## 3. Constraints
Locked:
- one-shot. No text, no logo, no people, no extra props.

Editable:
- one-shot. The comma-separated prompt above is the only artifact.

Append-only:
- one-shot. No result log.

Human approval:
- one-shot. Any later retouch, upscale spend, or publication is a
  separate decision.

Proof required:
- one-shot

Stop-and-ask:
- one-shot

Budgets:
- one-shot. One generation pass. Do not run a variation loop inside
  this prompt.

Enforce outside the prompt:
- The tool's own text and logo filters, if available.

## 4. Eval loop
Deterministic:
- one-shot. Reject the image if it contains text, a logo, a person,
  or a prop other than the carafe and the stone.

Rubric:
- Subject match 0.4: one matte black carafe, low and wide.
- Light and surface 0.4: morning side light, limestone, soft shadow.
- Restraint 0.2: empty background, no type.
- Passing bar 0.7.
- No model judge. A person accepts or rejects the still.

Keep / discard:
- one-shot. Keep a still that passes the checks above. Discard the
  rest. Do not iterate inside this package.

Stop when:
- one-shot. One image is delivered.

Log:
- one-shot. No log.

Target: Midjourney
Why: The ask is a single descriptive still, and Midjourney takes comma-separated descriptors rather than an edit loop.
```
