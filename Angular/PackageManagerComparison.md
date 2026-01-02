# pnpm

@pnpmjs is a strong option for protecting against supply chain attacks, and the developer experience is excellent — they removed postinstall scripts a while back.

- Boosted installation speed
- Creates a non-flat `node_modules` directory by default

When installing dependencies with npm or Yarn Classic, packages are hoisted to the root `node_modules`, so source code can access dependencies that are not declared. pnpm uses symlinks and a content-addressable store to add only the project’s direct dependencies into the project root.

## What is pnpm?

pnpm is a drop-in replacement for npm. It is built on top of npm and is faster and more efficient. It is highly disk-efficient and addresses several issues present in npm.

## Why not npm or Yarn?

npm’s flattened `node_modules` can cause issues such as:

- Complex algorithm for flattening dependency trees
- Duplicated packages inside other projects’ `node_modules`
- Modules having access to packages they do not depend on

---

## 1. npm (Node Package Manager)

### Pros

- Default: Comes pre-installed with Node.js; no extra install required.
- Popularity: Large ecosystem and community support.
- Improved performance: Since npm 5, includes `package-lock.json` for consistent installs and caching for speed.
- Security: Auditing features to check for vulnerabilities.

### Cons

- Slower install times compared to pnpm and Yarn.
- `node_modules` duplication across projects can use a lot of disk space.
- Older versions had dependency resolution issues (largely fixed in recent versions).

---

## 2. Yarn

### Pros

- Faster installs: Parallel installations and caching improve speed.
- Workspaces: Good monorepo support for managing multiple packages.
- Lock files: Uses `yarn.lock` to ensure consistent installs; sometimes more reliable than `package-lock.json`.
- Offline mode: Can install previously downloaded packages without internet.

### Cons

- Bigger initial install size (separate package manager).
- Slightly less community support compared to npm in some areas.
- Configurations (especially workspaces) can be more complex for some developers.

---

## 3. pnpm (Performant npm)

### Pros

- Super fast: Uses a global content-addressable store to avoid duplicating packages, reducing disk usage and install time.
- Efficient disk usage: Symlinks point to a single store rather than copying packages per project.
- Strict dependency resolution: Enforces strict `node_modules` structure so dependencies are only accessible if declared.
- Workspaces: Supports workspaces for monorepos like Yarn.

### Cons

- Compatibility: Some older packages may have issues with pnpm’s stricter module resolution.
- Learning curve: Different behavior from npm/Yarn can be confusing initially.
- Smaller community: Less widespread support and fewer resources than npm/Yarn.

---

## References

- https://refine.dev/blog/pnpm-vs-npm-and-yarn/#introduction
- https://pnpm.io/feature-comparison
