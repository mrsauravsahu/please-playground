package_base_name=split_path(package_name())[-1]
package=package_name()

genrule(
  name="docker_buildx",
  binary = True,
  cmd='''
    echo '#!/bin/sh' > $OUT
    echo 'IMAGE_TAG_VERSION="${VERSION:-latest}"' >> $OUT
    echo 'IMAGE_REPO="${VERSION:-{package_base_name}}"' >> $OUT
    echo 'PLATFORMS="${PLATFORMS:-linux/arm64,linux/amd64}"' >> $OUT
    echo 'EXTRA_ARGS="${EXTRA_ARGS}"' >> $OUT
    echo 'docker buildx build --platform "${PLATFORMS}" --build-arg APP={package_base_name} --build-arg RUST_VERSION=${RUST_VERSION} --build-arg DOTNET_RUNTIME_VERSION=${DOTNET_RUNTIME_VERSION} -t ${IMAGE_REPO}:${IMAGE_TAG_VERSION} -f plz-out/subrepos/plugins/docker-buildx/docker_buildx/docker/rust-cron-alpine.prod.dockerfile ${EXTRA_ARGS} {package}' >> $OUT
  '''.format(package_base_name=package_base_name, package=package),
  out="docker-buildx.sh"
)
