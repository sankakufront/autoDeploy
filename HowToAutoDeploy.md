#How to Auto Deploy

#リモートリポジトリからVHへ鍵認証で接続
##VH
.ssh/authorized_keysにpub_key追加

	.ssh 700
	authorized_keys	600
	
	mkdir -m 700 .ssh
	vi authorized_keys
	chmod 600 .ssh/authorized_keys
	

##リモートリポジトリ	※設定済み
.ssh/sec_key配置
	sec_key	400

##接続確認
	ssh -i .ssh/rep2 VHUSER@HOSTNAME -p2222


#VHからリモートリポジトリへ鍵認証で接続
##VH
.ssh/sec_key配置
.ssh/configに追記 (600)
	
	vi .ssh/config
	chmod 600 .ssh/config
	-----------------------------------------------
	Host            remoteRepos
	HostName        HOSTNAME
	User            repos
	Port            2222
	IdentityFile    /home/VHUSER/.ssh/rep2
	-----------------------------------------------

##リモートリポジトリ	※設定済み
.ssh/authorized_keys

##接続確認
	ssh remoteRepos

#VHでgit clone
##VH
    git clone ssh://remoteRepos/home/repos/REPOSUSER/repos.git
    mv repos/.git /home/VHUSER/.git
    rm -rf repos
	

##gitのリモートURLを確認
git remote -v

	==================================================================
	origin  ssh://remoteRepos/home/repos/REPOSUSER/repos.git (fetch)
	origin  ssh://remoteRepos/home/repos/REPOSUSER/repos.git (push)
	==================================================================

#リモートリポジトリでhook設定
##リモートリポジトリ
    cd REPOSUSER/repos.git/hooks/
    cp p post-receive.sample post-receive
    vi post-receive
	
	ssh -i /home/repos/.ssh/rep2 VHUSER@HOSTNAME -p2222 "cd ~;git pull"
	
#VHでgitignore編集
##VH
    .bash_history
    .bash_logout
    .bash_profile
    .bashrc
    .htpasswd
    .ssh/
    .viminfo
    logs
    www/.htaccess
