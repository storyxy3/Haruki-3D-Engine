# PJSK Character Shading Notes

This records the current IDA-backed boundary for the engine character shading work.

## Confirmed CharacterModel Material Writes

PJSK JP 6.5.5 `libil2cpp` IDA session `340297f3` was inspected at:

- `Sekai.Core.CharacterModel::SetupHairShadow` `0x7282B95A6198`
- `Sekai.Core.CharacterModel::UpdateSkinColor` `0x7282B95A667C`
- `Sekai.Core.CharacterModel::SetupFaceShadowAction` `0x7282B95A6CC4`
- `Sekai.Core.CharacterModel::UpdateShadowParameters` `0x7282B95AA844`
- `Sekai.Rendering.SekaiCharacterHair::Setup` `0x7282BEAA6410`
- `Sekai.Rendering.SekaiCharacterHair::OnUpdate` `0x7282BEAA6580`
- `Sekai.ShaderPropertyID.CharacterModelShaderPropertyId::.cctor` `0x7282BA340AF8`

The C# side confirms these high-level controls:

- Skin color update writes three material colors: default skin, shadow 1 skin, shadow 2 skin.
- Shadow parameter update writes `_ShadowTexWeight`, `_FadeMode`, and `_ShadowWidth`.
- Face shadow action writes the range-limit state and head/light values used by the face shadow limiter.
- Hair shadow setup toggles `_HAIR_SHADOW` and `_LAMBERT`.
- Hair update writes `_HeadPosition`.

No dedicated game-side `neck`, `chin`, `jaw`, `throat`, `contact`, or occlusion material controller was found in the `CharacterModel` shadow paths. The fixed darkening around neck, back head, and face-covered lower areas should therefore be treated as shader asset, authored texture, vertex data, or GPU shader behavior until proven otherwise.

## Engine Policy

The current `neckContact` path in `sekaiCharacterShader.ts` is experimental debug instrumentation only. It must not become production shading by changing a strength value or reusing the existing ellipse formula. Production work should instead continue from real shader assets or GPU capture evidence.

