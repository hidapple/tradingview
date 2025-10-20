# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a TradingView Pine Script repository for technical analysis indicators. Pine Script is TradingView's domain-specific language for creating custom technical indicators, strategies, and studies.

## Architecture

- `movings/` - Contains moving average indicators and related technical analysis scripts
- Pine Script files use `.pine` extension and follow Pine Script v6 syntax

## Pine Script Development

Pine Script files are text-based indicators that run on TradingView's platform. Key characteristics:
- Each script starts with `//@version=6` directive
- Use `indicator()` function to define script metadata
- Technical analysis functions are available under `ta.` namespace (e.g., `ta.sma()` for simple moving average)
- Plotting functions use `plot()` with customizable colors and line styles
- **All comments must be written in English only** - never use Japanese or other languages in code comments

## Code Structure Patterns

Pine Script indicators in this repository follow this pattern:
1. License header with MPL 2.0 reference
2. Version directive and indicator declaration
3. Calculate technical analysis values using `ta.*` functions
4. Plot results with descriptive titles and color coding

## Color Scheme Convention

Moving average indicators use consistent color coding:
- SMA 10: Cyan (`color.rgb(0, 255, 255)`)
- SMA 21: Pink (`color.rgb(255, 192, 203)`)
- SMA 50: Blue (`color.blue`)
- SMA 200: Red (`color.red`)

## Testing and Deployment

Pine Script indicators are tested and deployed directly within the TradingView platform. There are no traditional build, lint, or test commands for this repository type.
