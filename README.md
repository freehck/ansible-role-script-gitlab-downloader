freehck.script_gitlab_downloader
=========

This role copies the script that allows you to download build artifacts from gitlab.

Very useful for crontab jobs. Run it with `--help` to learn the options.

Role Variables
--------------
`gitlab_downloader_script_dir`: directory to install the script, default "/opt/scripts"

`gitlab_downloader_script_name`: script name, default "gitlab-downloader"

`gitlab_downloader_install_deps`: install dependencies, default `true`

Example Playbook
----------------

    - hosts:
        - webserver
      become: true
      vars:
        webroot_dir: "/var/www/html"
	gitlab_server: "gitlab.myorg.com"
	gitlab_project: "my-cool-website"
	gitlab_project_pipeline_id: "123456"
	gitlab_user_token: "<secret>"
	gitlab_artifact_path: "public/index.html"
	output_file: "{{ webroot_dir }}/index.html"
      roles:
        - role: freehck.script_gitlab_downloader
	  gitlab_downloader_script_dir: "/opt/scripts"
	  gitlab_downloader_script_name: "gitlab-downloader"
	  gitlab_downloader_install_deps: true
	- role: freehck.mkdir
	  mkdir_directories:
	    - "{{ webroot_dir }}"
      tasks:
        - name: download webroot
	  shell: >-
	    /opt/scripts/gitlab-downloader
	      -s {{ gitlab_server }}
	      -p {{ gitlab_project }}
	      -b {{ gitlab_project_pipeline_id }}
	      -k {{ gitlab_user_token }}
	      -f {{ gitlab_artifact_path }}
	      -o {{ output_file }}

License
-------
MIT

Author Information
------------------
Dmitrii Kashin, <freehck@freehck.com>
