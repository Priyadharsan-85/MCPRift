# Release process

MCPRift publishes from a version tag through GitHub Actions and PyPI Trusted
Publishing. The workflow does not store a PyPI API token.

## One-time repository setup

1. Confirm that the `mcprift` name is still available on PyPI. Availability is
   not a reservation.
2. In PyPI, create a pending Trusted Publisher for a new project, or add a
   Trusted Publisher to an existing project you own. Use owner `sanjayy0612`,
   repository `MCPRift`, workflow `publish-to-pypi.yml`, and environment `pypi`.
3. In GitHub, create the `pypi` environment and require manual approval from a
   maintainer. Do not add a PyPI token secret.
4. Ensure GitHub code scanning is available for the repository so the
   authorization workflow can accept its SARIF upload.

## Prepare a release

1. Set `src/mcprift/__init__.py` to the new semantic version. The build metadata
   and both labs read this value automatically.
2. Move the pending changelog entries under that version and add the release
   date.
3. Run the complete local gate:

   ```sh
   uv sync --locked
   uv run python -m unittest discover -s tests -v
   uv run ruff format --check .
   uv run ruff check .
   uv build
   uvx --from twine==7.0.0 twine check dist/*
   uv run mcprift demo
   ```

4. Review both archives. The wheel must contain the Python package, entry point,
   metadata, and license. The source archive must also contain the README,
   changelog, release guide, documentation, tests, and test data. Neither archive
   may contain `legacy-go`, `.github`, local artifacts, or credentials.
5. Commit the release, push it, and wait for the authorization workflow to pass.

## Publish

Create a new annotated tag. Never move or reuse an existing release tag.

```sh
git tag -a v0.5.0 -m "MCPRift 0.5.0"
git push origin v0.5.0
```

The tag starts the publish workflow. Its build job verifies that the tag and
package versions match, reruns the full static and unit-test gate, builds both
distributions, and validates their metadata. Review the run and approve the
`pypi` environment only if those checks pass.

After publication, install into a clean environment and verify:

```sh
uvx --from mcprift==0.5.0 mcprift version
uvx --from mcprift==0.5.0 mcprift demo
```

Then create the GitHub release from the same tag using the matching changelog
section. If publication fails after PyPI accepts a file, increase the version;
PyPI does not permit replacing an existing distribution.
