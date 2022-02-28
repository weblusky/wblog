### 动态更换视频地址时遇到的问题

最近在做一个动态查看视频（.mp4）文件的功能
根据链接点击，在video插件里播放不同的视频

video 部分
```Js
const TagMp4 = (props)=>{
	 const mp4Path = props.path.replace('.jpg', '.mp4')
	 const mp4style = {'maxHeight': '600px', 'maxWidth': '600px'}
	 return <video controls style={mp4style}>
		 <source src={mp4Path} type="video/mp4" ></source>
		 <p>Your browser doesn't support HTML5 video.</p>
	 </video>
}
```

显示 部分
```Js
const ShowVinfo = (props)=>{
	 const mp4 = props.vpath
	 const mp4info = <TagMp4 path={mp4} />
	 return <div className='txMp4'>
		 {mp4info}
	 </div>
}
```

当显示部分传递不同视频地址时，在开发工具里能看到地址的更换，但浏览器却不会播放新的视频，大概是video只变更属性，不会重新渲染播放，需要重新渲染整个video代码

于是调整了代码，在return时，增加了 key 属性
```Js
const ShowVinfo = (props)=>{
	 const mp4 = props.vpath
	 const mp4info = <TagMp4 path={mp4} />
	 return <div className='txMp4' key={mp4}>
		 {mp4info}
	 </div>
}
```

这样，随着地址的更新，video所在的DOM整个重新渲染，视频播放更新了。