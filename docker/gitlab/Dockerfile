FROM gitlab/gitlab-ce:14.1.2-ce.0

RUN sed -i '/^.*user_params\[:password_expires_at\] = Time.current if admin_making_changes_for_another_user.*$/s/^/#/' /opt/gitlab/embedded/service/gitlab-rails/lib/api/users.rb
