## Instalation
1. Install tutor, check [this](https://raccoongang.atlassian.net/wiki/spaces/~7120205d20c3e537594d258e217135f9976955/pages/3948052511/Tutor+Local+Development#12.-Mount-custom-file-to-container) guide if needed
2. Mount [RG edx-platform](https://gitlab.raccoongang.com/owlox-team/productsforge/oex/edx-platform)
```
# go to folder, where you store mounts
cd *your folder for mounts*

# clone edx-platform there
git clone https://gitlab.raccoongang.com/owlox-team/productsforge/oex/edx-platform

# go back to root folder of project
cd ..

# add mount
tutor mounts add *path to mounts folder*/edx-platform

# rebuild openedx image
tutor images build openedx
```
3. Install [RG OpenEdx plugin](https://gitlab.raccoongang.com/owlox-team/productsforge/oex/rg-openedx-plugin)
```
# go to private requirements (from tutor root folder)
cd env/build/openedx/requirements

# add line below into private.txt (create if it does't exist)
-e git+https://gitlab.raccoongang.com/owlox-team/productsforge/oex/rg-openedx-plugin@master#egg=rg-openedx-plugin

# rebuild openedx image
tutor images build openedx
```
4. Set supported languages inside settings file (`lms/envs/common.py`)
`MODELTRANSLATION_LANGUAGES = ('en', 'uk', 'es')`
5. Go to LMS admin panel, there you should see a new section called `EDX INFO PAGES`![[Screenshot 2024-11-21 at 23.52.14.png]]
6. First you need to go inside `Page types` and check if your page type already there (default pages like `tos`, `privacy`, `about`, `blog`, etc must already be)
	If it is, go to step 8.
7. Add custom page type
	 Create new page type with desirable slug![[Pasted image 20241122000432.png]]
	Then add your slug into `MKTG_URL_LINK_MAP` dict inside settings file
	```# lms/envs/common.py
	  ...
	  MKTG_URL_LINK_MAP = {
		  'ABOUT': 'about',
		  ...
		  'ABC': 'abc',
	  }
	  ```
8. Go to `Info pages` and create new one:
	1. Select page language from dropdown![[Pasted image 20241122001915.png]]
	2. Select what page type you want to change![[Pasted image 20241122002028.png]]
	3. Select `Site` where page will be placed (usually it's LMS)
	4. Provide `Title` which will be shown in browser tab and on page (don't forget about multilang support)![[Screenshot 2024-11-22 at 00.21.59.png]]
	5. Same with `Text` field, but it's more flexible, you can put images/videos, tables and even html code inside. All this will be displayed as content for your info page.![[Screenshot 2024-11-22 at 00.24.06.png]]
	6. You can save your page and go to `*lms-link*/your-page-type`, for tutor it would be like: `http://local.edly.io:8000/tos`