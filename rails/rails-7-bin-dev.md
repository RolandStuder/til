# Rails 7 provides a `bin/dev` for running in dev mode

Rails 7 depending on your CSS and js choice, will create a `bin/dev` file that installs/starts foreman
in order to cover your rebuilding needs during development.

This covers things like doing your esbuild, or rebuilding your Tailwind CSS.

btw: Do not run `rails assets:precompile`. At least with importmaps it then made my app use the compiled js in development 
and I had to run `rails assets:precompile` to get my updates stimulus controllers. In that case just delete everything
in `public/assets`
