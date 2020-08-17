DATE:2020.08.17.MON

Normally when we release own iOS opensources with cocoapod, we have to use pod commands in terminal interface Especially before push to cocoapods trunk, we have to use $ Pod lib lint command in order to check our pod’spec file correctly

I found an error case while I was working on new version release of my opensource, after excuted $ Pod lib lint command it showed error like below

    - NOTE  | xcodebuild:  error: SWIFT_VERSION '3.0' is unsupported, supported versions are: 4.0, 4.2, 5.0. (in target 'App' from project 'App')

    -> SummerSlider (0.4.0)
        - ERROR | [iOS] xcodebuild: Returned an unsuccessful exit code. You can use `--verbose` for more information.
        - NOTE  | xcodebuild:  note: Using new build system
        - NOTE  | xcodebuild:  note: Building targets in parallel
        - NOTE  | [iOS] xcodebuild:  note: Planning build
        - NOTE  | [iOS] xcodebuild:  note: Constructing build description
        - NOTE  | xcodebuild:  error: SWIFT_VERSION '3.0' is unsupported, supported versions are: 4.0, 4.2, 5.0. (in target 'App' from project 'App')
        - NOTE  | [iOS] xcodebuild:  warning: Skipping code signing because the target does not have an Info.plist file and one is not being generated automatically. (in target 'App' from project 'App')
        - NOTE  | xcodebuild:  error: SWIFT_VERSION '3.0' is unsupported, supported versions are: 4.0, 4.2, 5.0. (in target 'SummerSlider' from project 'Pods')

    [!] SummerSlider did not pass validation, due to 1 error.
    You can use the `--no-clean` option to inspect any issue.


The cause of error case is an existence of .swift-version file which to set up swift version for libary But it already deprecated and don’t need it anymore

To fix the error, just delete the .swift-version file

if you want to set up swift compileversion on your libary, just setup into pod file about . swift_version and see below sample :)

E.g
    Pod::Spec.new do |spec|
    spec.name          = 'Reachability'
    spec.version       = '3.1.0'
    spec.license       = { :type => 'BSD' }
    spec.homepage      = 'https://github.com/tonymillion/Reachability'
    spec.authors       = { 'Tony Million' => 'tonymillion@gmail.com' }
    spec.summary       = 'ARC and GCD Compatible Reachability Class for iOS and OS X.'
    spec.source        = { :git => 'https://github.com/tonymillion/Reachability.git', :tag => 'v3.1.0' }
    spec.module_name   = 'Rich'
    spec.swift_version = '4.0'

    spec.ios.deployment_target  = '9.0'
    spec.osx.deployment_target  = '10.10'

    spec.source_files       = 'Reachability/common/*.swift'
    spec.ios.source_files   = 'Reachability/ios/*.swift', 'Reachability/extensions/*.swift'
    spec.osx.source_files   = 'Reachability/osx/*.swift'

    spec.framework      = 'SystemConfiguration'
    spec.ios.framework  = 'UIKit'
    spec.osx.framework  = 'AppKit'

    spec.dependency 'SomeOtherPod'
    end