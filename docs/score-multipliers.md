# score multipliers

these changes start with alpha.17. they change actual score and the mod-select preview together. they do not change star ratings, pp, hit windows, map difficulty application or the server's pp policy.

## difficulty adjust

DA starts at 1.00x. enabling it without changing the map is neutral. each setting is compared with the original map, so setting OD 10 on an OD 10 map earns nothing extra.

- osu: the original/selected circle-radius ratio raised to 0.75, the original/selected great hit-window ratio raised to 0.60, and the original/selected approach-time ratio raised to 0.35.
- catch: the same size and approach-time weights, using catcher scale. OD is neutral because catch has no accuracy hit window. spicy patterns contribute 1.06x when enabled.
- taiko: the original/selected great hit-window ratio raised to 0.60. scroll speed stays neutral; it is a reading preference.
- mania: the original/selected perfect hit-window ratio raised to 0.60.
- HP in every mode contributes `exp(0.02 * HP change)`, deliberately much less than aiming or accuracy.

these are explicit score-balance weights, not a claim to measure every player's perceived difficulty. higher AR shortens approach time but is not universally harder for every pattern. its weight is lower than size and accuracy.

factors multiply, so easier changes offset harder ones. the DA component is bounded to 0.10x–4.00x. physical sizes/times have positive floors at extreme values. hit-window rounding follows the ruleset: changing a slider without changing the actual window earns no extra timing bonus.

examples, changing one setting at a time:

| change | score multiplier |
| --- | ---: |
| CS 4 to 6, osu or catch | 1.2354x |
| OD 8 to 10, osu | 1.3334x |
| AR 9 to 10, osu or catch | 1.1059x |
| HP 6 to 8 | 1.0408x |
| unchanged DA | 1.0000x |

## other inconsistencies fixed

- osu DT/NC at 1.01x now increases score instead of receiving a non-default-rate penalty. rates use continuous curves instead of rounding down in 0.1x chunks.
- HT/DC approach 1.00x score as speed approaches normal. default 0.75x bonuses/penalties remain the same: 0.55x for osu, 0.30x for the other modes. mania's existing neutral DT/NC policy is unchanged because its timing windows compensate for track rate.
- Wind Up/Down use 80% of the easier endpoint's multiplier plus 20% of the harder endpoint's multiplier. all-faster ramps give a bonus; all-slower ramps lose score. pitch changes are neutral.
- osu/taiko/catch Flashlight keeps a bonus with custom settings. the default bonus is divided by `size^0.75`, capped at +0.50. disabling combo-based shrinking divides that bonus by five. smaller visibility earns more, larger visibility earns less. osu's Freeze Frame combination still halves the visibility bonus.
- Approach Different is neutral: it changes the approach-circle animation, not the targets or judgement rules.
- selecting a mania key-count mod matching the map's original key count is neutral. actual key-count conversions retain their existing factor.

## intentionally retained

No Fail, Easy, Relax, Autopilot, Spun Out, slower playback, adaptive speed, magnetised aiming, pattern simplification, removal of releases/holds, constant-speed assistance, and timing-colour hints still have their existing assistance penalties unless explicitly covered above. mirror and other already-neutral visual transformations remain neutral. Hard Rock, Hidden, classic scoring and legacy score conversion retain their existing policies.

## existing scores

new gameplay uses this policy. stored/replayed scores opt into it only when their client version identifies a numbered Zigcho alpha.17-or-newer build. older Zigcho, official and unknown versions retain their original multiplier during score normalisation. no existing scores or player stats are rewritten by this client change.

CI checks current setting sweeps, mode-specific behaviour, mixed assistance mods, actual one-hit score processing, and the original upstream multiplier tables as historical-score compatibility tests.
