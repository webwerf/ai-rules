- Prioritize the "Repository Pattern combined with Domain-Driven Design (DDD)" pattern.
- Prioritize template strings for all strings, even when they don't contain a variable.
- Always write code in American English (eg "organization", "color", etc)
- Prefer the "Options Object Pattern" for optional options. Required parameters can just be parameters. One or 2 required paramers is fine, if we need at least 3, use 1 object.
- Ignore any linter errors about the "using" keyword.

# Comments

- When editing code, don't remove commented code or comments

# WebHare-specific

- In backend TypeScript code, prioritize the "import * as ... from ..." style when importing from `@webhare/*`.  Use the last file name for the import name, eg `import * as wrd from "@webhare/wrd"`. Use `import { x, y } from ...` in frontend TypeScript code (recognizable by being in path `/webdesigns/`).
-- For database-related functions (beginWork, commitWork, isWorkOpen) use `import * as whdb from '@webhare/whdb';`
-- For env-related functions (eg getDtapStage()), use `import * as env from "@webhare/env";`

## Tests

### Run tests

When the user asks to add tests somewhere, after the changes:

1. First check if there's a test name comment in the file (usually at the top in format `wh runtest <name>`).
2. If found, ask the user if they want to run the tests using that exact command:
   ```bash
   ~/projects/webhare/whtree/bin/wh runtest <found-name>
   ```
3. If not found, ask the user if they want to run the tests, and ask them to provide the test name:
   ```bash
   ~/projects/webhare/whtree/bin/wh runtest <name-of-test>

   What's the name of the test you'd like to run?
   ```

This way we can be specific when possible, but still handle cases where the test name isn't documented in the file.

### test.eq

Use test.eq to test expected value and result value. The function is defined as `function eq<T>(expected: NoInfer<RecursiveOrRegExp<T>>, actual: T, annotation?: Annotation): void`, eg first the expected value, then the actual value. Always set a short, concise annotation (3rd param) explaining the test.

# Errors

- For errors like `Student with email "${data.email}" already exists`, always add quotation marks (") around the variable. Prefix IDs with `#`, for example `Could not find campaign #${id}`.

# Classes

- In TypeScript, prefer classes if they make any sense
- ordering in a class:
1. class fields
2. constructor
3. getters/setters
4. public functions
5. private functions
- Use "private properties" for private fields and methods. Eg `#myPrivateFunction` instead of `private myPrivateFunction`. This only works in classes!

- Use "public" for public fields and functions, eg `public async <function>` and `public  <function>

# TypeScript

- For grouping, use object.groupBy instead of reduce

## Importing

When importing files, prefer the "mod-" approach above relative paths. Only if it's possible.

✅ Good:
import * as util from '@mod-mobieclick/webdesigns/mobieclick/shared/js/utilities';

❌ Avoid:
import { runDialog } from '../../components/dialog/dialog';

# TypeScript boolean conversion

Never use the double-bang operator (!!) for boolean conversion, always use Boolean()

✅ Good:
- Boolean(value)

❌ Avoid:
- !!value

# TypeScript array types

- When writing TypeScript array types:
-- Use `Array<T>` syntax for complex types (objects, unions, intersections)
-- Use `T[]` syntax only for simple types (string, number, boolean)

Example:
✅ Good:
- Simple: string[]
- Complex: Array<{ id: number; name: string }>
- Complex: Array<string | number>

❌ Avoid:
- Complex: { id: number; name: string }[]
- Complex: (string | number)[]

# TypeScript arrays

- In TypeScript, apply the "Require consistently using either T[] or Array<T> for arrays." rule.
✅ Good:
const x: string[] = ['a', 'b'];
const y: readonly string[] = ['a', 'b'];
const messages: cmApi.MessageRequest[] = tasks.map
❌ Bad:
const x: Array<string> = ['a', 'b'];
const y: ReadonlyArray<string> = ['a', 'b'];
const messages: Array<cmApi.MessageRequest> = tasks.map

# TSDoc

- When writing TSDoc documentation:
-- For @throws tags, never use curly braces. Write as "@throws Error" not "@throws {Error}"
-- For @param tags, always include a hyphen after the parameter name. Write as "@param name - description" not "@param name description"
-- For nested parameters (like `options.x`), document the parent parameter only. Don't try to document nested properties.
-- Example:
✅ Good:
* @param options - Query options containing outputColumns for selection
❌ Bad:
* @param options - Query options
* @param options.outputColumns - Columns to select

# Snake case object keys

In TypeScript, when building objects in WebHare task functions (which can be identified by ALL of these characteristics):
1. They are `export async function`s
2. Their first parameter derives from TaskRequest
3. They are typically found in files under `.../tasks/` or with `-tasks.ts` suffix
4. They are NOT methods in classes
Make sure to use snake_case for object keys, both in parameters as well as the function result. Eg don't use "skipValidation", but "skip_validation".
You can use `import { toSnakeCase, ToSnakeCase } from "@webhare/std/types";` to help with this. ToSnakeCase is both a function (`export function toSnakeCase`, "Convert all keys to camel case recursively") and a type (`export type ToSnakeCase`)

# Object Console Output

- When logging objects/data structures, prefer console.dir over JSON.stringify
- Always set depth to null to show full structure and enable colors
✅ Good:
console.dir(data, { depth: null, colors: true });

❌ Avoid:
console.log('Data:', JSON.stringify(data, null, 2));

# SCSS

We use BEM notation for styling, but we don't to use nesting. Eg this is okay:

.block {
  ...
}
.block__element {
  ...
}
.block__element--modifier {
  ...
}

This is not okay:

.block {
  &__element {
    &--modifier {
      ...
    }
  }
}

This is because we want to find any styling from browser inspectors easily.

The only nesting we want is for specificity where we override styling for components. Eg in a "faq" component, when we use the "p-accordion" component, it's best to do:

.faq {
  .p-accordion {
    ...
  }
  .p-accordion__header {
    ...
  }
}

# File Operations
When removing files or directories:
1. ALWAYS use the trash command instead of permanent deletion
2. On macOS: Use `trash path/to/file`
3. NEVER use `rm` or `delete_file` unless explicitly requested by the USER
4. If moving to trash fails, inform the USER and ask how to proceed

# Emojis

Start any new session with an emoji of a golden retriever.
Use ✅ and ❌ emojis in script output logging if it's a "success" or an "error" message.