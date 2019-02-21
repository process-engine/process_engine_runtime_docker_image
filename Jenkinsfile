#!/usr/bin/env groovy

def cleanup_workspace() {
  cleanWs()
  dir("${WORKSPACE}@tmp") {
    deleteDir()
  }
  dir("${WORKSPACE}@script") {
    deleteDir()
  }
  dir("${WORKSPACE}@script@tmp") {
    deleteDir()
  }
}

properties(
  [
    pipelineTriggers(
      [
        upstream(upstreamProjects: 'process-engine_node-lts/process_engine_runtime/master,'
                                 + 'process-engine_node-lts/process_engine_runtime/develop',
                threshold: hudson.model.Result.SUCCESS)
      ]
    )
  ]
);

NODE_VERSIONS = [
  '8-alpine',
  '10-alpine',
];

NODE_VERSION_FOR_LATEST_TAG = '10-alpine';

IMAGE_NAME = '5minds/process_engine_runtime';


def create_dockerfile_build_step(node_version, process_engine_version) {
  return {
    stage('Build and Publish') {

      def branch_is_master = BRANCH_NAME == 'master';

      def cleaned_branch_name = BRANCH_NAME.replace('/', '_');
      def base_image_tag = "${cleaned_branch_name}-b${BUILD_NUMBER}";

      def full_image_name = "${IMAGE_NAME}:${node_version}_${base_image_tag}";

      try {
        sh("docker build --build-arg NODE_IMAGE_VERSION=${node_version} \
                              --build-arg PROCESS_ENGINE_VERSION=${process_engine_version} \
                              --no-cache \
                              --tag ${full_image_name} .");

        def docker_image = docker.image(full_image_name);

        withDockerRegistry([ credentialsId: "5mio-docker-hub-username-and-password" ]) {

          docker_image.push("${node_version}-${cleaned_branch_name}");

          if (branch_is_master) {
            docker_image.push("${node_version}-latest");

            if (node_version == NODE_VERSION_FOR_LATEST_TAG) {
              docker_image.push('latest');
            }
          }

          if (node_version == NODE_VERSION_FOR_LATEST_TAG) {
            docker_image.push(cleaned_branch_name);
          }
        }
      } finally {
        sh("docker rmi ${full_image_name} || true");
      }
    }
  }
}

node('docker') {
  checkout scm;

  def branch_is_master = BRANCH_NAME == 'master';

  def dockerfile_builds = [:];

  def process_engine_version = branch_is_master ? 'latest' : 'develop';


  NODE_VERSIONS.each {

    dockerfile_builds[it] = create_dockerfile_build_step(it, process_engine_version);

  };

  parallel dockerfile_builds;

  cleanup_workspace();
}
