# mac-setup

Thêm một stack tùy biến trên dock để chứa tập tin, ứng dụng vừa mở . 
`defaults write com.apple.dock persistent-others -array-add '{"tile-data" = {"list-type" = 1;}; "tile-type" = "recents-tile";}'; killall Dock`

Thêm khoảng trắng trên dock
`defaults write com.apple.dock persistent-apps -array-add '{"tile-type"="spacer-tile";}'; killall Dock`

Reset thanh dock lại mặc định ban đầu
`defaults delete com.apple.dock; killall Dock`

Làm quen với các phím tắt trên Mac với phần mềm CheatSheet
