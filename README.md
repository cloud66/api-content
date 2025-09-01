# Cloud66 API Documentation Guide

This directory contains the MDX-based documentation for the Cloud66 API. This guide explains how to create, edit, and structure documentation files.

## Directory Structure

```
src/docs/
├── v3/                          # Current API version
│   ├── home.mdx                 # Version homepage
│   ├── getting-started/         # Getting started guides
│   ├── reference/               # API endpoint documentation
│   │   ├── stacks/
│   │   │   ├── 01_model.mdx     # Data model definition
│   │   │   ├── list.mdx         # GET /stacks
│   │   │   ├── get.mdx          # GET /stacks/:id
│   │   │   └── ...
│   │   └── ...
│   └── sdks/                    # SDK documentation
├── v2/                          # Legacy API version (deprecated)
└── v1/                          # Legacy API version (deprecated)
```

## File Types

### 1. Endpoint Documentation Files

These document individual API endpoints and must include specific frontmatter:

```mdx
---
title: List Stacks
scope: public
path: /stacks
method: GET
response: |
  {
    "response": [...],
    "count": 1,
    "pagination": {...}
  }
parameters: []
---

Brief description of what this endpoint does.

## Additional Details

Any additional information, examples, or notes about the endpoint.
```

**Required frontmatter fields:**
- `title`: Human-readable name for the endpoint
- `scope`: Access level (`public`, `admin`, `read-only`, etc.)
- `path`: API endpoint path
- `method`: HTTP method (`GET`, `POST`, `PUT`, `DELETE`, `PATCH`)
- `response`: Example JSON response (use YAML literal block `|`)
- `parameters`: Array of parameter definitions (can be empty `[]`)

### 2. Model Definition Files

These define data structures and are typically named `01_model.mdx`:

```mdx
---
---

### Resource Name

Brief description of the resource.

<Model>
    <ModelProperty name="property_name" type="string">
        Description of the property.
    </ModelProperty>
    <ModelProperty name="another_property" type="integer">
        Description of another property.
    </ModelProperty>
</Model>
```

### 3. General Content Files

For guides, getting started content, and other documentation:

```mdx
---
title: Page Title
---

# Content Title

Regular markdown content here.
```

## Naming Conventions

### File Naming
- **Endpoints**: Use lowercase with hyphens: `list.mdx`, `get.mdx`, `create.mdx`, `update.mdx`, `delete.mdx`
- **Models**: Use `01_model.mdx` (the numerical prefix ensures it appears first in navigation)
- **General content**: Use descriptive names with hyphens: `authentication.mdx`, `getting-started.mdx`

### URL Generation
- Numerical prefixes (e.g., `01_`, `02_`) are automatically stripped from URLs
- `01_model.mdx` becomes `/model` in the URL
- Underscores and spaces are converted to hyphens in URLs

## Folder Organization

### Reference Documentation
Organize by resource type:
```
reference/
├── stacks/
│   ├── 01_model.mdx
│   ├── list.mdx
│   ├── get.mdx
│   ├── create.mdx
│   ├── update.mdx
│   └── delete.mdx
├── servers/
├── deployments/
└── ...
```

### Folder Aggregation
When a folder contains multiple MDX files, they are automatically aggregated into a single page with:
- Individual anchor links for each file
- Collapsible navigation sections
- Proper SEO and metadata handling

## Available MDX Components

The following custom components are available for use in MDX files:

### Model Components

Use these components to define and document data structures:

#### `<Model>`
Container for model property definitions. Automatically styled with proper spacing and dividers.

```mdx
<Model>
    <!-- ModelProperty components go here -->
</Model>
```

#### `<ModelProperty>`
Defines individual properties within a model. 

**Props:**
- `name` (string, required): The property name
- `type` (string, required): The data type (e.g., "string", "integer", "boolean", "array", "object")

```mdx
<Model>
    <ModelProperty name="uid" type="string">
        The unique identifier for the resource.
    </ModelProperty>
    <ModelProperty name="count" type="integer">
        The total number of items returned.
    </ModelProperty>
    <ModelProperty name="is_active" type="boolean">
        Whether the resource is currently active.
    </ModelProperty>
    <ModelProperty name="tags" type="array">
        An array of tags associated with the resource.
    </ModelProperty>
    <ModelProperty name="metadata" type="object">
        Additional metadata object containing custom fields.
    </ModelProperty>
</Model>
```

### Callout Components

Use callouts to highlight important information, warnings, or additional context:

#### `<Callout>`
Creates styled alert boxes with icons for different types of information.

**Props:**
- `type` (string, required): The callout type - `"info"`, `"warning"`, or `"error"`
- `title` (string, required): The title text for the callout
- `children` (optional): Additional content inside the callout

```mdx
<Callout type="info" title="Important Information">
    This is additional context that users should be aware of.
</Callout>

<Callout type="warning" title="Be Careful">
    This action cannot be undone once performed.
</Callout>

<Callout type="error" title="Known Issue">
    There is a current limitation with this endpoint that affects performance.
</Callout>
```

**Visual Examples:**
- **Info**: Blue styling with info icon - for helpful tips and additional information
- **Warning**: Amber styling with warning triangle - for important caveats and considerations  
- **Error**: Red styling with X circle - for known issues, limitations, or critical warnings

### Enhanced Table Support

Tables are automatically enhanced with responsive styling and Shadcn components:

```mdx
| Property | Type | Description |
|----------|------|-------------|
| uid | string | Unique identifier |
| name | string | Display name |
| active | boolean | Status flag |
```

Tables automatically include:
- Responsive horizontal scrolling on mobile devices
- Consistent styling with the design system
- Proper spacing and typography
- Enhanced readability with alternating row colors

### Standard Markdown Features

All standard markdown and GitHub Flavored Markdown features are supported:

#### Code Blocks with Syntax Highlighting
```javascript
const response = await fetch('/api/stacks');
const data = await response.json();
```

```bash
curl -X GET "https://api.cloud66.com/3/stacks" \
     -H "Authorization: Bearer your-token"
```

#### Lists and Task Lists
- Regular bullet points
- Numbered lists
- [x] Completed tasks
- [ ] Pending tasks

#### Text Formatting
- **Bold text**
- *Italic text*
- ~~Strikethrough text~~
- `Inline code`

#### Links and References
- [External links](https://example.com)
- Internal links to other pages
- Auto-generated heading anchors

### Component Usage Tips

1. **Model Documentation**: Always use `<Model>` and `<ModelProperty>` for consistent API documentation
2. **Information Hierarchy**: Use callouts sparingly for truly important information
3. **Callout Placement**: Place callouts near relevant content, not at the beginning or end of pages
4. **Type Consistency**: Use consistent type names across all ModelProperty components (e.g., always use "integer" not "int")
5. **Content Length**: Keep callout titles concise and content focused

## Best Practices

### Content Guidelines
1. **Be Concise**: Write clear, focused descriptions
2. **Use Examples**: Include realistic example data in responses
3. **Consistent Formatting**: Follow the established patterns for similar content
4. **Parameter Documentation**: Always document required vs optional parameters
5. **Error Scenarios**: Document common error responses when relevant

### Technical Guidelines
1. **Validate JSON**: Ensure all JSON examples are valid
2. **Consistent Scoping**: Use consistent scope values across similar endpoints
3. **Proper HTTP Methods**: Use correct HTTP methods for each operation
4. **Path Parameters**: Document path parameters in the endpoint path (e.g., `/stacks/{uid}`)

### File Organization
1. **Group Related Endpoints**: Keep related endpoints in the same folder
2. **Model Files First**: Use `01_model.mdx` to define the data structure before endpoints
3. **Logical Ordering**: Order endpoints logically (list, get, create, update, delete)
4. **Version Consistency**: Keep consistent structure across API versions

## Editing Workflow

### For New Endpoints
1. Create the folder structure if it doesn't exist
2. Add the model file (`01_model.mdx`) if needed
3. Create the endpoint file with proper frontmatter
4. Test the content locally with `npm run dev`
5. Run `npm run index-endpoints` to update search index

### For Existing Endpoints
1. Edit the appropriate MDX file
2. Update the response examples if the API has changed
3. Ensure frontmatter is still accurate
4. Test changes locally
5. Update search index if needed

## Search Integration

The documentation includes Algolia-powered search that indexes:
- All endpoint titles and content
- HTTP methods and paths
- Scopes and parameters
- Content from `/reference/` directories

After adding new endpoints, run `npm run index-endpoints` to update the search index.

## Version Management

- **Current Version**: v3 (actively maintained)
- **Legacy Versions**: v2 and v1 (deprecated, minimal updates)
- **New Features**: Always add to the current version first
- **Breaking Changes**: Document migration notes when needed

## Need Help?

- Check existing files for examples and patterns
- Refer to the main project README for development setup
- Use the development server (`npm run dev`) to preview changes
- The site automatically handles routing and navigation based on file structure