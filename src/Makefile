PKG = kmeans

$(PKG).gfn: $(PKG).inp $(PKG).spec $(PKG)_sample.inp $(PKG)_help.md
	gretlcli --makepkg $(PKG).inp


install: $(PKG).gfn
	echo pkg install $(PKG).gfn --local | gretlcli -b -
	cat $(PKG)_help.md | tee ../README.md

clean:
	rm -f $(PKG).gfn
