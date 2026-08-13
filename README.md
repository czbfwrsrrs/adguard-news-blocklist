# Custom Filter List

This is my personal AdGuard Home DNS blocklist for media sites, social platforms and related domains that I don't want accessible on my network.

## Where it lives

The working Git repo is on the HTPC at:

`/var/home/htpc/custom-filter-list`

The same location may also appear as:

`/home/htpc/custom-filter-list`

The blocklist itself is:

`/var/home/htpc/custom-filter-list/customfilter.txt`

The GitHub repo is:

`czbfwrsrrs/custom-filter-list`

## AdGuard Home

AdGuard Home runs on the AdGuard VM.

Its main config is:

`/usr/local/bin/AdGuardHome/AdGuardHome.yaml`

Downloaded filter copies are stored under:

`/usr/local/bin/AdGuardHome/data/filters/`

Those downloaded files are only AdGuard's cache, so I shouldn't edit them directly. Changes should be made in `customfilter.txt` in this Git repo and then pushed to GitHub.

## Rule format

I use AdGuard rules in this form:

`||example.com^`

For example:

`||foxnews.com^`

This blocks the main domain as well as subdomains such as `www.foxnews.com` and `prod-hp.foxnews.com`.

DNS filtering can't filter parts of a URL after the hostname, so rules such as `bbc.co.uk/news` won't work for blocking only one section of a website.

## Updating the list

```bash
cd /var/home/htpc/custom-filter-list
nano customfilter.txt

git add customfilter.txt
git commit -m "Update blocklist"
git push origin main
```

AdGuard Home will pick up the new GitHub copy on its normal filter refresh. If I want a change applied straight away, I can manually refresh the Personal Blocklist in AdGuard Home.

## Useful checks

Check that the repo is clean and the remote is correct:

```bash
cd /var/home/htpc/custom-filter-list
git status
git remote -v
```

Test a blocked domain directly against AdGuard:

```bash
dig +short @192.168.1.138 foxnews.com
dig +short @192.168.1.138 www.foxnews.com
dig +short @192.168.1.138 prod-hp.foxnews.com
```

With the current blocking mode, blocked domains should normally return `0.0.0.0`.
