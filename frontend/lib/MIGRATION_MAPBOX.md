# Mapbox GL JS Upgrade (v1.13.2 -> v3.x)

Date: 2025-09-28

## Summary
We upgraded `mapbox-gl` from `^1.13.2` to `^3.6.0` (actually resolved to a later patch `3.15.0` via Yarn) and `react-map-gl` from `^5.3.21` to `^7.1.x` to maintain compatibility.

## Key Code Changes
- Replaced `StaticMap` usage with the `Map` (aliased locally as `MapboxMap`) component from `react-map-gl@7` in `DeckGlJsonChart.tsx`.
- Removed `MapContext`/`ContextProvider` prop; not required with new API usage.
- Renamed prop `mapboxApiAccessToken` -> `mapboxAccessToken`.
- Added inline style to ensure map container sizing instead of inheriting from removed wrapper semantics.
- Set `interactive={false}` to preserve previous non-interactive StaticMap behavior (DeckGL still handles interaction for selection).

## Dependency Adjustments
- Removed `@types/mapbox-gl` devDependency: types are bundled with Mapbox GL JS >= v2.
- Updated `mapbox-gl` to `^3.6.0` (resolved higher patch by Yarn) and `react-map-gl` to `^7.1.9`.

## Potential Follow-Ups
- Evaluate bundle size impact: new dedicated chunks (`mapbox-gl-*`) increased size; consider dynamic import or conditional loading when mapbox layers actually used.
- Confirm license implications: Mapbox GL JS >= v2 switched to a proprietary commercial license (Mapbox Terms of Service). Ensure compliance with redistribution and token usage policies.
- Re-run visual regression/E2E tests involving DeckGL/Mapbox maps.
- Evaluate whether `interactive={false}` matches desired UX; if pan/zoom is wanted, remove and rely on DeckGL controller integration.

## Testing
- Ran `yarn install` and `yarn build` in `frontend/lib` – build succeeded.
- No TypeScript errors surfaced for updated map components.

## Notes
`deck.gl` layers still function through DeckGL's controller. If future issues arise with event handling, consider passing the `mapLib` and `mapboxAccessToken` via DeckGL props or exploring `deck.gl@latest` compatibility notes.
