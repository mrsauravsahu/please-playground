# name of package with out the prefix - hello_world in this case
package_base_name=split_path(package_name())[-1]

# name of the package from the root of the project - apps/hello_world in this case
package=package_name()

# tar ball the source files which will be used when creating the docker build
tarball(
    name="srcs",
    srcs=glob(["src", "Cargo*"]) + ["///please_reuse//docker_buildx/docker:rust-cron-alpine.prod.dockerfile"],
)

# genrule to run docker buildx to generate the docker image
genrule(
  name="docker_buildx",
  deps=[":srcs"],
  binary = True,
  pass_env=["VERSION", "DOTNET_VERSION", "DOTNET_RUNTIME_VERSION"],
  cmd='''
    echo '#!/bin/sh' > $OUT
    echo 'IMAGE_TAG_VERSION="${VERSION:-latest}"' >> $OUT
    echo 'PLATFORMS="${PLATFORMS:-linux/arm64,linux/amd64}"' >> $OUT
    echo 'EXTRA_ARGS="${EXTRA_ARGS}"' >> $OUT
    echo 'mkdir plz-out/gen/{package}/srcs' >> $OUT
    echo 'tar xvf plz-out/gen/{package}/srcs.tar.gz -C plz-out/gen/{package}/srcs' >> $OUT
    echo 'docker buildx build --platform "${PLATFORMS}" --build-arg RUST_VERSION=${RUST_VERSION} --build-arg APP={package_base_name} -t ghcr.io/{package}:${IMAGE_TAG_VERSION} -f plz-out/gen/{package}/srcs/rust-cron-alpine.prod.dockerfile ${EXTRA_ARGS} plz-out/gen/{package}/srcs' >> $OUT
  '''.format(package=package, package_base_name=package_base_name),
  out="docker-buildx.sh"
)
