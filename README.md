{
  "kb_name": "ai-dev-kb-2026",
  "version": "1.0.0",
  "updated": "2026-06-30",
  "philosophy": "CAPABILITY-BASED, NOT HARDCODED. Modules declare what they PROVIDE and what they CAN CONSUME. The engine connects any provider->consumer pair whose tags match. Never force a fixed A->B->C chain. If A can connect to B, allow it. ปรัชญา: ต่อกันได้ตามความเข้ากันได้ ไม่บังคับเส้นทางตายตัว",

  "linking_rule": {
    "type": "tag_match",
    "description": "Module X may connect to module Y if (X.provides ∩ Y.consumes) is non-empty. Multiple valid links are allowed; pick by context, not by hardcoded order.",
    "wildcard": "* matches any tag",
    "soft_links": "A link with weight < 1.0 is OPTIONAL — use only if it improves the result"
  },

  "global_constraints": {
    "max_file_size_mb": 25,
    "github_ready": true,
    "data_currency": "2026",
    "research_policy": "Always verify volatile facts (versions, pricing, APIs, deprecations) against the live web before relying on them. See 00-core/02-web-research-2026.md",
    "before_every_action": "Run the pre-flight error check. See 00-core/01-error-checking.md"
  },

  "modules": [
    {
      "id": "core.reasoning",
      "file": "00-core/00-reasoning-engine.md",
      "role": "Advanced reasoning for hard problems",
      "provides": ["reasoning", "decomposition", "tradeoff-analysis", "*"],
      "consumes": ["*"],
      "weight": 1.0,
      "always_on": true
    },
    {
      "id": "core.errorcheck",
      "file": "00-core/01-error-checking.md",
      "role": "Pre-flight + post-flight error / QA checking",
      "provides": ["validation", "qa", "safety-gate"],
      "consumes": ["*"],
      "weight": 1.0,
      "always_on": true
    },
    {
      "id": "core.research",
      "file": "00-core/02-web-research-2026.md",
      "role": "Force live web research for current facts",
      "provides": ["fresh-facts", "citations", "version-check"],
      "consumes": ["volatile-fact", "version", "pricing", "api-spec"],
      "weight": 1.0,
      "always_on": true
    },
    {
      "id": "core.compose",
      "file": "00-core/03-composition-rules.md",
      "role": "How modules link to each other (the graph engine spec)",
      "provides": ["composition", "routing"],
      "consumes": ["*"],
      "weight": 1.0,
      "always_on": true
    },
    {
      "id": "design.minimal",
      "file": "10-design/10-minimal-modern-2026.md",
      "role": "Advanced minimal / modern visual design (2026)",
      "provides": ["design-tokens", "layout", "typography", "color", "ui-spec", "motion"],
      "consumes": ["reasoning", "fresh-facts", "ui-target"],
      "weight": 1.0
    },
    {
      "id": "web.code",
      "file": "20-web/20-web-coding-2026.md",
      "role": "Web front/back coding (2026 stack)",
      "provides": ["web-ui", "web-api-client", "ui-target", "frontend"],
      "consumes": ["design-tokens", "layout", "typography", "color", "ui-spec", "motion", "api-endpoint", "auth", "data-store"],
      "weight": 1.0
    },
    {
      "id": "roblox.luau",
      "file": "30-roblox/30-roblox-luau-2026.md",
      "role": "Roblox Studio / Luau game code (2026)",
      "provides": ["game-logic", "game-ui", "remote-events", "client-server"],
      "consumes": ["reasoning", "validation", "api-endpoint", "data-store", "auth", "design-tokens"],
      "weight": 1.0
    },
    {
      "id": "backend.firebase",
      "file": "40-backend/40-firebase-api-server-2026.md",
      "role": "API + server management on Firebase (2026)",
      "provides": ["api-endpoint", "auth", "data-store", "server-logic", "realtime", "ai-logic"],
      "consumes": ["reasoning", "validation", "fresh-facts", "web-api-client", "game-logic"],
      "weight": 1.0
    },
    {
      "id": "meta.naming",
      "file": "90-meta/90-naming-conventions.md",
      "role": "Clear, fast naming conventions across all domains",
      "provides": ["naming", "structure"],
      "consumes": ["*"],
      "weight": 0.8
    },
    {
      "id": "meta.github",
      "file": "90-meta/91-github-packaging.md",
      "role": "GitHub packaging, <=25MB rule, repo layout",
      "provides": ["packaging", "repo-layout", "ci"],
      "consumes": ["naming", "structure", "*"],
      "weight": 0.8
    }
  ],

  "example_resolved_links": [
    "web.code  <- design.minimal      (design-tokens, layout, ui-spec)   [hard 1.0]",
    "web.code  <- backend.firebase    (api-endpoint, auth, data-store)   [hard 1.0]",
    "roblox.luau <- backend.firebase  (api-endpoint via HttpService, data-store) [hard 1.0]",
    "roblox.luau <- design.minimal    (design-tokens for GUI)            [soft 0.8 — optional]",
    "ALL      <- core.reasoning/errorcheck/research                      [always 1.0]"
  ],

  "anti_rules": [
    "DO NOT assume web.code must always use backend.firebase — only if a data-store/auth/api need exists.",
    "DO NOT force design.minimal onto roblox.luau — link only when a GUI is being built.",
    "DO NOT skip core.research for any version number, price, or API surface."
  ]
}
