---
layout: post
title:  "Closer to the deploy-in-one-step"
date:   2020-08-15 08:43:00 +0000
categories: deploy script automation
---

I've been trying out a deployment checklist which is a bit different than
usual. It's a little script which ~~helps me~~ _guides me_ through out the whole
process.

So, on my current project, deploying for staging environment is performed in
six(ish) steps:

1. Inform everyone deploy is about to begin.
2. On my local repository, switch to the `deploy` branch.
3. In it, execute the deploy command.
4. Turn on the VPN.
   4. Open the site and tryout a feature or two.
   4. Turn off the VPN.
5. Move cards forward.
6. Inform everyone deploy has ended.

This is too long. At most it should be steps 1 and 6, with a _Press deploy
button_ in between. Depending on what "inform everyone" means—email, text, chat,
yell—we can also automate that. And then we can finally make a ~~build~~
_deploy_ in one step.

My script performs steps 2, 3, 4, and 4.2 for me. 😊 But I still have to move
cards about on the board, and test is the site is up and running.  😕 For now,
maybe I can use a Slack command line client to inform everyone,

So, without further ado, the script. Sensitive data was replaced with King
Crimson(ish) references.

```sh
#!/bin/bash
set -eu

VPNNAME="PROJEKT-X-VPN"

config_elasticbeanstalk () {
    mkdir .elasticbeanstalk
    cd .elasticbeanstalk
    cat <<EOF > config.yml
…yadda yadda yadda…
EOF
    cd ..
}

run_elastic_beanstalk () {
    echo "Running Elastic Beanstalk…"
    config_elasticbeanstalk
    eb deploy projekt-staging
}

clone_repository () {
    echo "Cloning code repository…"
    rm -rf projekt-x
    git clone -q git@github.com:projekts/projekt-x.git
}

echo "Alert in #projekt-x the deploy is about to beging:"
echo "  started \`projekt-staging\` deploy"
read -n 1 -p "Press any key to continue: "

cd /tmp && clone_repository
cd projekt-x && run_elastic_beanstalk

networksetup -connectpppoeservice "${VPNNAME}"
open https://projekt-x-staging.projekts.com
echo "Confirm if everything is okay."
read -n 1 -p "Press any key to continue: "
networksetup -connectpppoeservice "${VPNNAME}"

echo "Move cards"
open https://projekts.board.com/board/101421138
read -n 1 -p "Press any key to continue: "

echo "Inform in #projekt-x the deploy has ended:"
echo "  finished \`projekt-staging\` deploy"
read -n 1 -p "Press any key to continue: "

echo "All done"
```

---


Inspiration for this script came from [here](https://blog.danslimmon.com/2019/07/15/do-nothing-scripting-the-key-to-gradual-automation/ "Do-nothing scripting, by Dan Slimmon").

The goal of a _one build step_ was incited by [him](https://www.joelonsoftware.com/2000/08/09/the-joel-test-12-steps-to-better-code/ "Joel Spolsky")
