Originally tried:
npm install -g @anthropic-ai/claude-cli                                                                                                                                          
claude --help
⠙(node:32390) Warning: Setting the NODE_TLS_REJECT_UNAUTHORIZED environment variable to '0' makes TLS connections and HTTPS requests insecure by disabling certificate verification.
(Use `node --trace-warnings ...` to show where the warning was created)
npm error code E404
npm error 404 Not Found - GET https://registry.npmjs.org/@anthropic-ai%2fclaude-cli - Not found
npm error 404
npm error 404  The requested resource '@anthropic-ai/claude-cli@*' could not be found or you do not have permission to access it.
npm error 404
npm error 404 Note that you can also install from a
npm error 404 tarball, folder, http url, or git url.
npm error A complete log of this run can be found in: /home/hiranp/.npm/_logs/2025-11-04T20_08_59_774Z-debug-0.log

The after JFROG config:

Even tried the process:
npm install @anthropic-ai/claude-cli --registry https://jfrog.accenturefederaldev.com/artifactory/api/npm/afs-npm-proxy-remote/                                                       
npm warn Unknown user config "email". This will stop working in the next major version of npm.
npm warn Unknown user config "always-auth". This will stop working in the next major version of npm.
⠙(node:4050) Warning: Setting the NODE_TLS_REJECT_UNAUTHORIZED environment variable to '0' makes TLS connections and HTTPS requests insecure by disabling certificate verification.
(Use `node --trace-warnings ...` to show where the warning was created)
npm error code E404
npm error 404 Not Found - GET https://jfrog.accenturefederaldev.com/artifactory/api/npm/afs-npm-proxy-remote/@anthropic-ai%2fclaude-cli - not_found
npm error 404
npm error 404  The requested resource '@anthropic-ai/claude-cli@*' could not be found or you do not have permission to access it.
npm error 404
npm error 404 Note that you can also install from a
npm error 404 tarball, folder, http url, or git url.
npm error A complete log of this run can be found in: /home/hiranp/.npm/_logs/2025-11-04T20_22_04_746Z-debug-0.log

npm install -g @anthropic-ai/claude-cli                                                                                                                                               
claude --help
npm warn Unknown user config "email". This will stop working in the next major version of npm.
npm warn Unknown user config "always-auth". This will stop working in the next major version of npm.
npm error code ERR_INVALID_AUTH
npm error Invalid auth configuration found: `_auth` must be renamed to `//jfrog.accenturefederaldev.com/artifactory/api/npm/afs-npm-proxy/:_auth` in user config
npm error Please run `npm config fix` to repair your configuration.`
npm error A complete log of this run can be found in: /home/hiranp/.npm/_logs/2025-11-04T20_17_16_687Z-debug-0.log
zsh: command not found: claude
