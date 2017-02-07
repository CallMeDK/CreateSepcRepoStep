#Podspec语法
参阅
<a herf="http://guides.cocoapods.org/syntax/podspec.html#specification">Podspec Syntax Reference</a>

podspec文件必须的字段：
1.name
2.version
3.authors
4.social_media_url
5.license
6.homepage
7.source
8.summary

<h3>非ARC 与 ARC</h3>
requires_arc allows you to specify which source_files use ARC. This can either be the files which support ARC, or true to indicate all of the source_files use ARC.

<pre><code>
util.subspec 'file' do |file|

  file.source_files = "YT_TKFMWK/Classes/THFMWK/util/file/**/*.{h,m}"



  file.requires_arc = false

end
</code></pre>

<h3>frameworks</h3>
1.系统的
A list of system frameworks that the user’s target needs to link against.

<pre><code>
spec.ios.framework = 'CFNetwork'

  spec.frameworks = 'QuartzCore', 'CoreData
  
</code></pre>

2.第三方的
<pre><code>
 spec.vendored_frameworks = 'MyFramework.framework', 'TheirFramework.framework'
</code></pre>

<h3>libraries</h3>
1.系统的
<pre><code>
spec.ios.library = 'xml2'

 spec.libraries = 'xml2', 'z'
 
</code></pre>
2.第三方的
<pre><code>
 spec.ios.vendored_library = 'Libraries/libProj4.a'

 spec.vendored_libraries = 'libProj4.a', 'libJavaScriptCore.a'
 
</code></pre>

<h3>resource_bundles</h3>
<pre><code>
spec.ios.resource_bundle = { 'MapBox' => 'MapView/Map/Resources/*.png' }

spec.resource_bundles = {

    'MapBox' => ['MapView/Map/Resources/*.png'],

    'OtherResources' => ['MapView/Map/OtherResources/*.png']

}
</code></pre>

<h3>
resources【不是很推荐使用】
</h3>
We strongly recommend library developers to adopt resource bundles as there can be name collisions using the resources attribute. Moreover, resources specified with this attribute are copied directly to the client target and therefore they are not optimised by Xcode.
<pre><code>
spec.resource = 'Resources/HockeySDK.bundle'

spec.resources = ['Images/*.png', 'Sounds/*']

</code></pre>

###Subspec
<pre><code>
subspec 'Twitter' do |sp|

  sp.source_files = 'Classes/Twitter'

end

subspec 'Pinboard' do |sp|

  sp.source_files = 'Classes/Pinboard'

end
</code></pre>

依赖
<pre><code>
Pod::Spec.new do |s|

  s.name = 'RestKit'

  s.subspec 'Core' do |cs|

cs.dependency 'RestKit/ObjectMapping'
cs.dependency 'RestKit/Network'
cs.dependency 'RestKit/CoreData'

  end

  s.subspec 'ObjectMapping' do |os|

  end

end

</code></pre>

嵌套
<pre><code>
Pod::Spec.new do |s|

  s.name = 'Root'

  s.subspec 'Level_1' do |sp|
sp.subspec 'Level_2' do |ssp|
end
end
end

</code></pre>

