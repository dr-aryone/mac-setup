# mac-setup

Install [Homebrew](https://brew.sh/) and [Cask](https://github.com/Homebrew/homebrew-cask)

Clone this repo
```sh
mkdir projects
cd projects
git clone https://github.com/thanhtoan1196/mac-setup.git && cd mac-setup
```

Install casks

```sh
while read app; do
  brew cask install "$app"
done < homebrew-cask/basic.txt
```



```sh
# Remove downloaded files and cleanup space
brew cask cleanup
```



Install Mac App Store apps

```sh
brew install mas # cli for the mac app store
```

```sh
app_ids_without_comments=$(grep -o '^[^#]*' mas/basic.txt | grep -v '^$')
echo $app_ids_without_comments | xargs mas install
```



Development



Install homebrews

```sh
while read tool; do
  brew install "$tool"
done < homebrew/dev.txt
```



Install casks

```sh
while read app; do
  brew cask install "$app"
done < homebrew-cask/dev.txt
```

Install [Oh My Zsh](https://github.com/robbyrussell/oh-my-zsh)

Thêm một stack tùy biến trên dock để chứa tập tin, ứng dụng vừa mở  
`defaults write com.apple.dock persistent-others -array-add '{"tile-data" = {"list-type" = 1;}; "tile-type" = "recents-tile";}'; killall Dock`

Thêm khoảng trắng trên dock  
`defaults write com.apple.dock persistent-apps -array-add '{"tile-type"="spacer-tile";}'; killall Dock`

Reset thanh dock lại mặc định ban đầu  
`defaults delete com.apple.dock; killall Dock`

Làm quen với các phím tắt trên Mac với phần mềm CheatSheet
