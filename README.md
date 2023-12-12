# test_repo2



require 'ffi'

module WinMM
  extend FFI::Library
  ffi_lib 'winmm'

  # ヘッダファイルの内容を参照しながら必要な関数を定義する
  attach_function :mciSendString, :mciSendStringA, [:string, :pointer, :int, :int], :uint
end

# 音楽を再生する関数
def play_music(file_path)
  command = "open \"#{file_path}\" type mpegvideo alias MyMusic"
  WinMM.mciSendString(command, nil, 0, 0)

  command = 'play MyMusic'
  WinMM.mciSendString(command, nil, 0, 0)
end

# バックグラウンドで音楽を再生するスレッド
bg_thread = Thread.new do
  play_music('path/to/your/music.mp3')
end

# ここにLibUIのウィンドウの作成と他のUIコードを記述する