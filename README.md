# docker-image-publisher
Github Action to build and push tagged Docker image to a Registry.

## Usage

### Push to Docker Hub

```yaml
permissions:
  contents: read
  packages: write
  id-token: write
  attestations: write

jobs:
  build-n-push-image:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Build and Push Image to Docker Hub
        uses: kjuly/docker-image-publisher@main
        with:
          image_name: user/image
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          token: ${{ secrets.DOCKERHUB_TOKEN }}
```
The format of `image_name` is "user/image", which is optional here. If you don't provide one, `${{ github.repository }}` will by used.

For `DOCKERHUB_USERNAME` & `DOCKERHUB_TOKEN`, go to the repository's "Settings > Security > Secrets and variables > Actions > Repository secrets" and add your Docker Hub username and token there.

See [Create and manage access tokens][create-n-manage-access-tokens] for the details about generating Docker Hub tokens.

---
### Push to Github Container Registry

```yaml
...

- name: Build and Push Image to Github Container Registry
  uses: kjuly/docker-image-publisher@main
  with:
    registry: ghcr.io
    image_name: ${{ env.IMAGE_NAME }}
    username: ${{ github.actor }}
    token: ${{ secrets.GITHUB_TOKEN }}
```

[create-n-manage-access-tokens]: https://docs.docker.com/security/for-developers/access-tokens/
