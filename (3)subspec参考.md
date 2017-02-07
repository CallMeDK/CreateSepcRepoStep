#Subspec 参考

这个话题，需要多看其他开源库的podspec

##参考1：

<pre><code>
Pod::Spec.new do |s|

  s.name             = "PodTestLibrary"

  s.version          = "1.0.0"

  s.summary          = "Just Testing."

  s.description      = <<-DESC

                       Testing Private Podspec.

                       * Markdown format

.

                       * Don't worry about the indent, we strip it!

                       DESC

  s.homepage         = "https://coding.net/u/wtlucky/p/podTestLibrary"

  # s.screenshots     = "www.example.com/screenshots_1", "www.example.com/screenshots_2"

  s.license          = 'MIT'

  s.author           = { "wtlucky" => "wtlucky@foxmail.com" }

  s.source           = { :git => "https://coding.net/wtlucky/podTestLibrary.git", :tag => "1.0.0"

}

  # s.social_media_url = 'https://twitter.com/<TWITTER_USERNAME>'

  s.platform     = :ios, '7.0'

  s.requires_arc = true

  #s.source_files = 'Pod/Classes/**/*'

  #s.resource_bundles = {

  #  'PodTestLibrary' => ['Pod/Assets/*.png']

  #}

  #s.public_header_files = 'Pod/Classes/**/*.h'

  s.subspec 'NetWorkEngine' do |networkEngine|

      networkEngine.source_files = 'Pod/Classes/NetworkEngine/**/*'

      networkEngine.public_header_files = 'Pod/Classes/NetworkEngine/**/*.h'

      networkEngine.dependency 'AFNetworking', '~> 2.3'

  end

  s.subspec 'DataModel' do |dataModel|

      dataModel.source_files = 'Pod/Classes/DataModel/**/*'

      dataModel.public_header_files = 'Pod/Classes/DataModel/**/*.h'

  end

  s.subspec 'CommonTools' do |commonTools|

      commonTools.source_files = 'Pod/Classes/CommonTools/**/*'

      commonTools.public_header_files = 'Pod/Classes/CommonTools/**/*.h'

      commonTools.dependency 'OpenUDID', '~> 1.0.0'

  end

  s.subspec 'UIKitAddition' do |ui|

      ui.source_files = 'Pod/Classes/UIKitAddition/**/*'

      ui.public_header_files = 'Pod/Classes/UIKitAddition/**/*.h'

      ui.resource = "Pod/Assets/MLSUIKitResource.bundle"

      ui.dependency 'PodTestLibrary/CommonTools'

  end

  s.frameworks = 'UIKit'

  #s.dependency 'AFNetworking', '~> 2.3'

  #s.dependency 'OpenUDID', '~> 1.0.0'

end
</code></pre>

##参考2：
<pre><code>
Pod::Spec.new do |s|

  s.name         = "iOS_Util"

  s.version      = "0.10.0"

  s.summary      = "Some iOS Util"

  s.license      = 'MIT'

  s.author       = { "文通" => "wentong@taobao.com" }

  s.platform     = :ios, '6.1'

  s.ios.deployment_target = '4.3'

  s.subspec 'Common' do |cos|

    cos.source_files = 'iOS_Util/Common/*.{h,m}'

    cos.public_header_files = 'iOS_Util/Common/*.h'

  end

  s.subspec 'Core' do |cs|

    cs.source_files = 'iOS_Util/Core/*.{h,m}'

    cs.public_header_files = 'iOS_Util/Core/*.h'

    cs.dependency 'libextobjc', '0.2.5'

  end

  s.subspec 'Json' do |js|

    js.source_files = 'iOS_Util/Json/*.{h,m}'

    js.public_header_files = 'iOS_Util/Json/*.h'

    js.dependency 'iOS_Util/Core'

  end

  s.subspec 'Bean' do |bs|

    bs.source_files = 'iOS_Util/Bean/*.{h,m}'

    bs.public_header_files = 'iOS_Util/Bean/*.h'

    bs.dependency 'iOS_Util/Core'

  end

  s.subspec 'DB' do |ds|

    ds.source_files = 'iOS_Util/DB/*.{h,m}'

    ds.public_header_files = 'iOS_Util/DB/*.h'

    ds.dependency 'FMDB/standard' ,'~> 2.1'

    ds.dependency 'iOS_Util/Common'

    ds.dependency 'iOS_Util/Core'

  end

  s.subspec 'WebP' do |ws|

    ws.source_files = 'iOS_Util/WebP/*.{h,m}'

    ws.public_header_files = 'iOS_Util/WebP/*.h'

    ws.dependency 'libwebp' ,'~> 0.3.0-rc7'

    ws.frameworks = 'CoreGraphics'

  end

  s.subspec 'Location' do |ls|

    ls.source_files = 'iOS_Util/Location/*.{h,m}'

    ls.public_header_files = 'iOS_Util/Location/*.h'

    ls.dependency 'iOS_Util/Common'

    ls.dependency 'iOS_Util/DB'

    ls.frameworks = 'CoreLocation' ,'MapKit'

    ls.resource = 'iOS_Util/Location/chinaDivision.sqlite'

  end

  s.subspec 'AMR' do |as|

    as.source_files = 'iOS_Util/AMR/**/*.{h,m,mm}'

    as.public_header_files = 'iOS_Util/AMR/**/*.h'

    as.preserve_paths = "iOS_Util/AMR/**"

    as.library   = 'opencore-amrnb','opencore-amrwb'

    as.xcconfig  = { 'LIBRARY_SEARCH_PATHS' => '"$(PODS_ROOT)/iOS_Util/iOS_Util/AMR/lib"' }

  end

  s.subspec 'Cache' do |cas|

    cas.source_files = 'iOS_Util/Cache/*.{h,m,mm}'

    cas.public_header_files = 'iOS_Util/Cache/*.h'

    cas.dependency 'iOS_Util/Common'

  end

  s.subspec 'Preference' do |ps|

    ps.source_files = 'iOS_Util/Preference/*.{h,m,mm}'

    ps.public_header_files = 'iOS_Util/Preference/*.h'

    ps.dependency 'iOS_Util/Json'

  end

  s.requires_arc = true

end
</code></pre>

##参考3：
<pre><code>
Pod::Spec.new do |s|

  s.name = 'SDWebImage'

  s.version = '3.8.2'

  s.ios.deployment_target = '7.0'

  s.tvos.deployment_target = '9.0'

  s.license = 'MIT'

  s.summary = 'Asynchronous image downloader with cache support with an UIImageView category.'

  s.homepage = 'https://github.com/rs/SDWebImage'

  s.author = { 'Olivier Poitrey' => 'rs@dailymotion.com' }

  s.source = { :git => 'https://github.com/rs/SDWebImage.git', :tag => s.version.to_s }

  s.description = 'This library provides a category for UIImageView with support for remote '      \

'images coming from the web. It provides an UIImageView category adding web '    \

'image and cache management to the Cocoa Touch framework, an asynchronous '      \

'image downloader, an asynchronous memory + disk image caching with automatic '  \

'cache expiration handling, a guarantee that the same URL won\'t be downloaded ' \

'several times, a guarantee that bogus URLs won\'t be retried again and again, ' \

'and performances!'

  s.requires_arc = true

  s.framework = 'ImageIO'

  s.default_subspec = 'Core'

  s.subspec 'Core' do |core|

    core.source_files = 'SDWebImage/{NS,SD,UI}*.{h,m}'

    core.exclude_files = 'SDWebImage/UIImage+WebP.{h,m}'

    core.tvos.exclude_files = 'SDWebImage/MKAnnotationView+WebCache.*'

end

  s.subspec 'MapKit' do |mk|

    mk.ios.deployment_target = '7.0'

    mk.source_files = 'SDWebImage/MKAnnotationView+WebCache.*'

    mk.framework = 'MapKit'

    mk.dependency 'SDWebImage/Core'

end

  s.subspec 'WebP' do |webp|

    webp.source_files = 'SDWebImage/UIImage+WebP.{h,m}'

    webp.xcconfig = {

'GCC_PREPROCESSOR_DEFINITIONS' => '$(inherited) SD_WEBP=1',

'USER_HEADER_SEARCH_PATHS' => '$(inherited) $(SRCROOT)/libwebp/src'

    }

    webp.dependency 'SDWebImage/Core'

    webp.dependency 'libwebp'

 end

end
</code></pre>

