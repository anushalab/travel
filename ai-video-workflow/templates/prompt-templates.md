# Prompt Templates

## Keyframe (image) prompt
```
[SHOT TYPE] of [SUBJECT + key details], [ACTION/POSE], in [LOCATION], [TIME OF DAY / LIGHTING].
Composition: [rule of thirds / centered / leading lines], [foreground element].
STYLE BLOCK
```

## Video (image-to-video) prompt — describe MOTION, not the picture
```
Camera: [one move — slow dolly in / orbit left / crane up / static tripod / handheld follow].
Subject: [one clear action, with speed — e.g. "slowly turns her head toward camera and smiles"].
Environment: [secondary motion — drifting mist, flags moving in wind, balloons rising].
Pacing: [calm / energetic]. No cuts. Keep identity and outfit consistent.
```

## STYLE BLOCK (append to every prompt; edit once per project)
```
Cinematic, shot on ARRI Alexa, 35mm lens, shallow depth of field, natural skin texture,
warm golden-hour grade with teal shadows, subtle film grain, high dynamic range, photorealistic.
```

## Negative guidance (put into prompt as "avoid ...")
```
Avoid: warped hands, extra fingers, morphing faces, text artifacts, flicker, sudden cuts,
oversaturated colors, plastic skin.
```

## Example — travel hook shot
Keyframe:
```
Extreme wide shot of dozens of colorful hot-air balloons floating above the fairy chimneys of
Cappadocia at sunrise, a lone traveler in a beige linen shirt standing on a cliff edge in the
lower-left third, mist in the valleys. STYLE BLOCK
```
Motion:
```
Camera: slow crane up revealing the full valley. Subject: traveler stays still, wind moves
her shirt gently. Environment: balloons rise slowly, mist drifts. Calm pacing. No cuts.
```
