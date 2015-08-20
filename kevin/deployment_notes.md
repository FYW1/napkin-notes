# My Steps

## Quick Overview

The Baseline Setup is common to both Tutor and BigLearn installations - it isn't optional.  Once it's done, though, you can do either (or both) of the Installing Tutor and Installing BigLearn steps in any order.

## Baseline Setup

### Check Your umask

```
umask
```
should be `0022` (I think...).
Something like `0077` is definitely no good.

### [Optional] Clear Out Python Environments

```
rmvirtualenv bldemo
```

or the more brutal:

```
rm -rf ~/.environments/{bl*,tutordep}
```

### Create a Deployment Directory

```
mkdir deploy_yymmdd_hhmm
cd !$
```

### Create Local Copies of Repos

```
for i in accounts exchange exercises exercises-js tutor-js tutor-server biglearn-algs biglearn-demos biglearn-common biglearn-platform tutor-deployment ; do git clone http://github.com/openstax/$i ; done
```

### Setup tutordep Python Virtual Environment

```
cd tutor-deployment
mkvirtualenv -p /usr/bin/python2 tutordep --system-site-packages
workon tutordep
pip install -r requirements.txt
```

## Installing Tutor

### Run Deploy Tutor-Related Servers (except BigLearn - see below)

Note: If re-deploying, you can add:
```
--skip-tags "configuration"
```
to the commands below.

#### All Servers

Note the use of the environment name (`kev` in this case; customize as appropriate).

```
ansible-playbook -i environments/kev/inventory tutor.yml --vault-password-file ~/.vaultkev --private-key ~/.ssh/tutor_kev.pem
```

Skipping configuration:

```
ansible-playbook -i environments/kev/inventory tutor.yml --skip-tags "configuration" --vault-password-file ~/.vaultkev --private-key ~/.ssh/tutor_kev.pem
```

#### Tutor Only

```
ansible-playbook -i environments/kev/inventory tutor_only.yml --vault-password-file ~/.vaultkev --private-key ~/.ssh/tutor_kev.pem
```

Skipping configuration:

```
ansible-playbook -i environments/kev/inventory tutor_only.yml --skip-tags "configuration" --vault-password-file ~/.vaultkev --private-key ~/.ssh/tutor_kev.pem
```

## Installing BigLearn

### Create Python Virtual Environments

```
cd ../biglearn-algs
mkvirtualenv blalgs -p /usr/bin/python2 --system-site-packages
workon blalgs
pip install -e "../biglearn-common[quest, reqval]"
pip install -r requirements.txt
```

```
cd ../biglearn-platform/app
mkvirtualenv blapidev -p /usr/bin/python2 --system-site-packages
workon blapidev
pip install -e "../../biglearn-common[quest, reqval]"
pip install -e ../../biglearn-algs
pip install -r dev-requirements.txt
```

```
mkvirtualenv blapi -p /usr/bin/python2 --system-site-packages
workon blapi
pip install -e "../../biglearn-common[quest, reqval]"
pip install -e ../../biglearn-algs
pip install -r requirements.txt
mkdir uploads
```

```
cd ../../biglearn-demos/platform-demos
mkvirtualenv bldemos -p /usr/bin/python2 --system-site-packages
workon bldemos
pip install -e "../../biglearn-common[quest, reqval]"
pip install -r requirements.txt
```

### Re-join tutordep Environment (created above)

```
cd ../../tutor-deployment
workon tutordep
```

### Run BigLearn Deploy Playbook

Note the use of the environment name (`kev` in this case; customize as appropriate).

```
ansible-playbook -i environments/kev/inventory biglearn.yml --vault-password-file ~/.vaultkev --private-key ~/.ssh/tutor_kev.pem
```

# Other Interesting Stuff

```
ansible-playbook -i environments/kev/inventory create_ssh_config.yml --extra-vars "key_path=~/.ssh/tutor_kev.pem" --vault-password-file ~/.vaultkev

sh sshenv.sh kev biglearnflower
```

```
cat environments/kev/inventory|grep flower -A 3|grep ansible_ssh_host
```

`blapidev` is needed to generate the API documentation.