# openapi-diff-semver-bump
A GitHub Action for getting appropriate updated [SemVer](https://semver.org/) OpenAPI package version.

## Usage
The following example [workflow steps](https://help.github.com/en/actions/configuring-and-managing-workflows/configuring-a-workflow) will generate a new version by previous tag and updated state from [OpenAPITools/openapi-diff](https://github.com/OpenAPITools/openapi-diff).

```yml
- name: "Find difference between OpenAPI specifications"
  id: diff_state
  uses: swimmwatch/openapi-diff-action@v1.0.1
  with:
    old-spec: "old_spec.json"
    new-spec: "new_spec.json"
- name: "Get previous tag"
  id: previous_tag
  uses: WyriHaximus/github-action-get-previous-tag@v1
  env:
    GITHUB_TOKEN: ${{secrets.GITHUB_TOKEN}}
- name: "Get appropriate new package version"
  id: updated_version
  uses: swimmwatch/openapi-diff-semver-bump@v1
  with:
    package-version: ${{steps.previous_tag.outputs.tag}}
    updated-state: ${{steps.diff_state.outputs.state}}
```

## Options
The following input variables options can/must be configured:

|Input variable|Necessity|Description|Default|
|----|----|----|----|
|`package-version`|Required|Package [SemVer](https://semver.org/) version||
|`updated-state`|Required|Output state from [OpenAPITools/openapi-diff](https://github.com/OpenAPITools/openapi-diff): `no_changes`, `incompatible`, `compatible`||

## Outputs
- `new-package-version`: New package [SemVer](https://semver.org/) version e.g. `1.2.3`.

## License
openapi-diff-semver-bump is licensed under the [MIT License](LICENSE).