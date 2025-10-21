You are generating a VS Code breakpoint collection file for the Loukas Kotas 'Breakpoints Manager' extension. Your output must be a SINGLE valid JSON array (minified) that matches EXACTLY the schema below and will be written to the project root as "./breakpoints.json".

SCHEMA (must match exactly, including keys and shapes):

[

{

"name": "<collection-name>",

"guid": "<uuid-v4>",

"breakpoints": [

{

"enabled": true,

"location": "/<posix-path-from-workspace-root>",

"line": [

{"line": <0-based-line>, "character": 0},

{"line": <0-based-line>, "character": 0}

]

}

...

]

}

]

IMPORTANT: Line numbers are EXTREMELY IMPORTANT. Line numbers are EXTREMELY IMPORTANT. Before computing any line numbers, RELOAD the content of all relevant files from the current in-memory state (not from stale snapshots), because the user may have edited files after your last change.

OPTIONAL USER QUERY SECTION (If this section has text besides whitespace contained between the triple quotes, you will use mode a) described below. You will always consider mode b) if it is relevant. Modes a) and b) are not mutually exclusive):

"""

"""

OPTIONAL FILES CHANGED SECTION (This section may or may not have content, use it as the source of truth for mode b) please rather than your current context. Also, filepaths here are missing the **required** `/` prefix, keep that in mind when writing final filepaths in your output, the `/` prefix is **required**):

`text

`

OPTIONAL 

What to do:

1) Determine target breakpoints in ONE of two modes (pick whichever is applicable based on the current context you see):

a) USER-QUERY MODE: Traverse the repository to find code locations that satisfy the user’s intent:

- Use the query: "{{USER_QUERY}}"

- Search all source files: .ts, .tsx, .js, .jsx, .mjs, .cjs, .py, .go, .rb, .java, .cs, .php as applicable.

- Heuristics (apply as relevant to the query):

• For route handlers (e.g., Express, Fastify, Next.js API routes): set a breakpoint on the first executable statement inside the handler function body.

• For class or function definitions: set a breakpoint on the function’s first executable line (skip comments, decorators/annotations, import lines).

• For React components: set a breakpoint on the first executable statement inside the component body (skip imports and type-only lines).

• For event handlers and callbacks: set a breakpoint at the first statement of the callback.

• For pipeline/worker/queue handlers: set a breakpoint at the handler entry line.

• If a file matches multiple targets, add multiple breakpoint entries.

b) CHANGES MODE: If you have just modified files in this session, generate breakpoints at key entry points in those changed files:

- Reload each changed file’s LATEST content from memory.

- Identify the main exported functions, route handlers, class constructors, and top-level component bodies you touched.

- Place breakpoints at their first executable statements.

2) Path formatting:

- Use POSIX-style paths, relative to the workspace root, prefixed with a single leading slash. Example: "/src/api/routes/digests.ts"

- Do NOT include the absolute machine path.

3) Line/character coordinates:

- "line" must be an array of TWO objects with IDENTICAL values (start and end), each with "character": 0.

- Lines are ZERO-BASED indices.

- Never point to blank lines, import statements, or comments (unless the user query explicitly asks for that). Choose the first executable code line of the target.

4) Deduplication & validation:

- Remove duplicate breakpoints (same file + same line).

- Verify each target file still exists and includes the target symbol/statement.

- If a target cannot be found, omit it (do NOT invent lines).

5) Output contract:

- Output ONLY the final JSON array (minified). No Markdown, no comments, no trailing commas, no extra text.

- Use a human-friendly collection name and a valid UUID v4 for "guid".

- If no matching locations are found, emit an empty collection with "breakpoints": [].

Now perform the traversal/analysis, RELOAD the needed files from memory, compute exact zero-based line numbers, and output the single minified JSON array in the required schema to be written to "./breakpoints.json".
