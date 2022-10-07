ARG GITLAB_VERSION
FROM gitlab/gitlab-ce:${GITLAB_VERSION}
RUN sed -i '/^.*user_params\[:password_expires_at\] = Time.current if admin_making_changes_for_another_user.*$/s/^/#/' /opt/gitlab/embedded/service/gitlab-rails/lib/api/users.rb
