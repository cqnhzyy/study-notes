<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>周越的技术博客</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', 'Microsoft YaHei', sans-serif;
        }
        
        body {
            background-color: #f5f7fa;
            color: #333;
            line-height: 1.6;
            padding: 20px;
        }
        
        .container {
            max-width: 1000px;
            margin: 0 auto;
            background: white;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
            overflow: hidden;
        }
        
        header {
            background: linear-gradient(135deg, #1e5799 0%, #207cca 100%);
            color: white;
            padding: 30px;
            text-align: center;
        }
        
        header h1 {
            font-size: 2.2rem;
            margin-bottom: 10px;
        }
        
        header p {
            font-size: 1.1rem;
            opacity: 0.9;
        }
        
        .blog-container {
            padding: 30px;
        }
        
        .blog-post {
            margin-bottom: 50px;
            border-bottom: 1px solid #eee;
            padding-bottom: 30px;
        }
        
        .blog-post:last-child {
            border-bottom: none;
        }
        
        .post-title {
            color: #1e5799;
            font-size: 1.8rem;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 2px solid #f0f0f0;
        }
        
        .post-meta {
            color: #666;
            font-size: 0.9rem;
            margin-bottom: 20px;
            display: flex;
            align-items: center;
        }
        
        .post-date {
            background: #eef5ff;
            padding: 3px 10px;
            border-radius: 15px;
            margin-right: 15px;
        }
        
        .post-tags {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
        }
        
        .tag {
            background: #f0f7ff;
            color: #1e5799;
            padding: 3px 10px;
            border-radius: 12px;
            font-size: 0.8rem;
        }
        
        .post-content {
            margin-top: 20px;
        }
        
        .post-content h3 {
            color: #1e5799;
            margin: 20px 0 10px;
            font-size: 1.3rem;
        }
        
        .post-content p {
            margin-bottom: 15px;
        }
        
        .code-block {
            background: #2d2d2d;
            color: #f8f8f2;
            padding: 15px;
            border-radius: 5px;
            overflow-x: auto;
            margin: 15px 0;
            font-family: 'Consolas', monospace;
            font-size: 0.9rem;
            line-height: 1.5;
        }
        
        .code-comment {
            color: #75715e;
        }
        
        .code-keyword {
            color: #66d9ef;
        }
        
        .code-string {
            color: #e6db74;
        }
        
        .code-function {
            color: #a6e22e;
        }
        
        .result-box {
            background: #eef7ff;
            border-left: 4px solid #1e5799;
            padding: 15px;
            margin: 15px 0;
            border-radius: 0 5px 5px 0;
        }
        
        .tip-box {
            background: #fff9e6;
            border-left: 4px solid #ffc107;
            padding: 15px;
            margin: 15px 0;
            border-radius: 0 5px 5px 0;
        }
        
        footer {
            text-align: center;
            padding: 20px;
            background: #f0f5ff;
            color: #666;
            font-size: 0.9rem;
        }
        
        @media (max-width: 768px) {
            .blog-container {
                padding: 20px;
            }
            
            .post-title {
                font-size: 1.5rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>周越的技术博客</h1>
            <p>记录云计算学习笔记与项目实践</p>
        </header>
        
        <div class="blog-container">
            <!-- 博客文章1 -->
            <article class="blog-post">
                <h2 class="post-title">我是如何将静态网站加载速度提升56%的？—— 华为云CDN实战</h2>
                <div class="post-meta">
                    <span class="post-date">2024-05-10</span>
                    <div class="post-tags">
                        <span class="tag">华为云</span>
                        <span class="tag">CDN</span>
                        <span class="tag">性能优化</span>
                        <span class="tag">Nginx</span>
                    </div>
                </div>
                
                <div class="post-content">
                    <p>最近在部署个人项目网站时，发现页面加载特别慢，尤其是图片和CSS文件，动辄需要3-4秒。这体验太差了！决定用华为云的CDN（内容分发网络）来给它提速。</p>
                    
                    <h3>我的优化思路：</h3>
                    <p>1. <strong>动静分离</strong>：先把网站里的"动"（PHP程序）和"静"（图片、CSS、JS）分开。我的网站是静态的，所以直接把所有静态资源拎出来就行。</p>
                    <p>2. <strong>资源上传</strong>：在华为云上创建一个OBS桶，取名叫 <code>my-website-static</code>，然后把所有静态文件都传了上去。</p>
                    <p>3. <strong>接入CDN</strong>：在CDN控制台，将我的OBS桶作为源站，创建了一个加速域名 <code>cdn.cqnhzyy.example.com</code>。</p>
                    <p>4. <strong>修改网站配置</strong>：把网站代码里所有引用静态资源的地方，比如 <code>&lt;img src="/images/logo.png"&gt;</code>，全部改成指向CDN加速域名，如 <code>&lt;img src="https://cdn.cqnhzyy.example.com/images/logo.png"&gt;</code>。</p>
                    
                    <h3>遇到的坑 & 解决方案：</h3>
                    <p><strong>问题</strong>：修改后访问，图片显示403错误。</p>
                    <p><strong>排查</strong>：原来是OBS桶的权限没设对。默认是私有桶，CDN没法读取。</p>
                    <p><strong>解决</strong>：在OBS桶策略里，授权给CDN服务的域名读取权限。搞定！</p>
                    
                    <h3>效果验证：</h3>
                    <p>优化前，用Chrome浏览器开发者工具测试，首页完全加载平均需要 <code>3.2s</code>。</p>
                    <p>配置CDN并刷新缓存后，同样的页面加载时间降到了 <code>1.4s</code>！</p>
                    <div class="result-box">
                        <p><strong>计算一下：(3.2 - 1.4) / 3.2 ≈ 56%！</strong> 提升非常明显，目标达成！</p>
                    </div>
                    
                    <div class="tip-box">
                        <p><strong>心得：</strong> CDN的原理就是把我的静态资源复制到全国各地的节点，用户访问时就从离他最近的节点获取，速度自然就快了。这真是提升网站体验的性价比最高的手段之一。</p>
                    </div>
                </div>
            </article>
            
            <!-- 博客文章2 -->
            <article class="blog-post">
                <h2 class="post-title">云服务器安全入门：记一次华为云安全组配置实践</h2>
                <div class="post-meta">
                    <span class="post-date">2024-05-18</span>
                    <div class="post-tags">
                        <span class="tag">华为云</span>
                        <span class="tag">安全组</span>
                        <span class="tag">服务器安全</span>
                        <span class="tag">ECS</span>
                    </div>
                </div>
                
                <div class="post-content">
                    <p>今天在Cloud Eye上看到几条奇怪的登录尝试告警，虽然密码强度大，但还是吓出一身冷汗。立刻意识到服务器安全组配置可能太"奔放"了，得赶紧收紧一下。</p>
                    
                    <h3>我的安全组"最小权限"配置原则：</h3>
                    <p><strong>入方向规则（谁可以访问我的服务器）：</strong></p>
                    <p>1. <strong>规则1</strong>：允许 <code>SSH (22端口)</code>，但源IP不填 <code>0.0.0.0/0</code>（全世界），而是只填我自己的办公网络IP段 <code>123.123.123.0/24</code>。这样只有我能SSH上去。</p>
                    <p>2. <strong>规则2</strong>：允许 <code>HTTP (80端口)</code> 和 <code>HTTPS (443端口)</code>，源IP是 <code>0.0.0.0/0</code>。因为网站需要被所有人访问。</p>
                    <p>3. <strong>规则3</strong>：<strong>拒绝所有</strong> 其他入站流量。这是默认规则，确保没被明确允许的端口一律不通。</p>
                    
                    <p><strong>出方向规则（我的服务器可以访问谁）：</strong></p>
                    <p>默认允许所有出站流量，方便系统更新和安装软件。</p>
                    
                    <h3>操作步骤：</h3>
                    <p>在ECS控制台，找到我的云服务器所属的安全组，把原来的"允许所有入方向"的规则删除，然后添加上面两条精确的入方向规则。</p>
                    
                    <h3>配置完的直观感受：</h3>
                    <p>1. 用手机热点（非办公IP）尝试SSH连接，果然被拒绝了，安全感瞬间提升。</p>
                    <p>2. 网站访问一切正常。</p>
                    <p>3. Cloud Eye里的暴力破解告警过一会就消失了，因为攻击者连敲门的机会都没有了。</p>
                    
                    <div class="tip-box">
                        <p><strong>心得：</strong> 云服务器的安全组就像是云端的虚拟防火墙，是安全的第一道也是最重要的一道防线。一定要遵循"最小权限原则"，需要什么开什么，千万不要图省事全开放。</p>
                    </div>
                </div>
            </article>
            
            <!-- 博客文章3 -->
            <article class="blog-post">
                <h2 class="post-title">零成本监控：利用华为云Cloud Eye守护我的云服务</h2>
                <div class="post-meta">
                    <span class="post-date">2024-05-25</span>
                    <div class="post-tags">
                        <span class="tag">华为云</span>
                        <span class="tag">Cloud Eye</span>
                        <span class="tag">监控</span>
                        <span class="tag">告警</span>
                    </div>
                </div>
                
                <div class="post-content">
                    <p>服务器和网站部署好了，但不能做"瞎子"，得知道它运行是否健康。华为云的Cloud Eye监控服务有免费额度，对我这种个人项目完全够用，今天就把它配置起来。</p>
                    
                    <h3>我重点关注的监控项：</h3>
                    <p>1. <strong>CPU使用率</strong>：超过80%就可能意味着程序有问题或者被攻击了。</p>
                    <p>2. <strong>磁盘使用率</strong>：万一日志把磁盘写满了，网站可就挂了。</p>
                    <p>3. <strong>站点连通性</strong>：定时检测我的网站首页是否能正常打开。</p>
                    
                    <h3>告警配置实战：</h3>
                    <p>1. <strong>CPU告警</strong>：</p>
                    <p>创建告警规则，选择我那台ECS。条件：<code>CPU使用率</code> <code>最大值</code> <code>&gt;= 80%</code>，持续 <code>2</code> 个周期（5分钟）。触发动作：发送邮件和短信通知我。</p>
                    
                    <p>2. <strong>磁盘告警</strong>：</p>
                    <p>条件：<code>磁盘使用率</code> <code>&gt;= 85%</code>。</p>
                    
                    <p>3. <strong>站点监控（超实用）</strong>：</p>
                    <p>在Cloud Eye的"站点监控"里，创建一个HTTP(S)测试任务，定时请求我的网站URL。设置告警：如果连续 <code>2</code> 次请求失败，就立刻通知我。</p>
                    
                    <h3>真实触发经历：</h3>
                    <p>上周就收到一条磁盘告警，显示使用率达到了87%。登录服务器一查，发现是Nginx的access.log增长过快。正好用上前几天写的Python日志清理脚本跑了一下，瞬间释放了空间，避免了一次潜在的服务中断。</p>
                    
                    <div class="tip-box">
                        <p><strong>心得：</strong> 监控是运维的眼睛。通过Cloud Eye设置合理的告警阈值，能让我在用户发现问题之前就感知并处理掉故障，实现"主动运维"。对于免费额度，真是"不用白不用"！</p>
                    </div>
                </div>
            </article>
            
            <!-- 博客文章4 -->
            <article class="blog-post">
                <h2 class="post-title">Python脚本实战：自动清理服务器日志文件</h2>
                <div class="post-meta">
                    <span class="post-date">2024-06-05</span>
                    <div class="post-tags">
                        <span class="tag">Python</span>
                        <span class="tag">Linux</span>
                        <span class="tag">运维脚本</span>
                        <span class="tag">日志管理</span>
                    </div>
                </div>
                
                <div class="post-content">
                    <p>上次磁盘告警后，手动清理日志太麻烦了。作为一个云运维学习者，自动化是必须的！写个Python脚本让服务器自己定期清理。</p>
                    
                    <h3>脚本功能设计：</h3>
                    <p>目标：清理 <code>/var/log/nginx/</code> 目录下超过7天的日志文件（<code>*.log</code>）。</p>
                    <p>方式：不是暴力删除，而是使用 <code>logrotate</code> 的思想，只清理旧的归档文件。</p>
                    
                    <h3>代码实现 (clean_old_logs.py):</h3>
                    <div class="code-block">
                        <span class="code-comment">#!/usr/bin/env python3</span><br>
                        <span class="code-comment">"""</span><br>
                        <span class="code-comment">自动清理旧日志文件脚本</span><br>
                        <span class="code-comment">功能：删除指定目录下，超过指定天数的 .log 文件</span><br>
                        <span class="code-comment">"""</span><br><br>
                        
                        <span class="code-keyword">import</span> os<br>
                        <span class="code-keyword">import</span> time<br>
                        <span class="code-keyword">import</span> argparse<br>
                        <span class="code-keyword">from</span> datetime <span class="code-keyword">import</span> datetime, timedelta<br><br>
                        
                        <span class="code-keyword">def</span> <span class="code-function">clean_old_logs</span>(directory, days):<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;<span class="code-comment">"""</span><br>
                        &nbsp;&nbsp;&nbsp;&nbsp;<span class="code-comment">清理指定目录中早于指定天数的.log文件</span><br>
                        &nbsp;&nbsp;&nbsp;&nbsp;<span class="code-comment">:param directory: 要清理的目录路径</span><br>
                        &nbsp;&nbsp;&nbsp;&nbsp;<span class="code-comment">:param days: 保留的天数</span><br>
                        &nbsp;&nbsp;&nbsp;&nbsp;<span class="code-comment">"""</span><br>
                        &nbsp;&nbsp;&nbsp;&nbsp;<span class="code-comment"># 计算临界时间点</span><br>
                        &nbsp;&nbsp;&nbsp;&nbsp;cutoff_time = time.time() - (days * 24 * 60 * 60)<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;total_freed_space = 0<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;deleted_files = []<br><br>
                        
                        &nbsp;&nbsp;&nbsp;&nbsp;print(<span class="code-string">f"开始在目录 [{directory}] 中清理 {days} 天前的 .log 文件..."</span>)<br><br>
                        
                        &nbsp;&nbsp;&nbsp;&nbsp;<span class="code-keyword">for</span> filename <span class="code-keyword">in</span> os.listdir(directory):<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="code-keyword">if</span> filename.endswith(<span class="code-string">'.log'</span>):<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;filepath = os.path.join(directory, filename)<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="code-comment"># 检查文件修改时间和大小</span><br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="code-keyword">if</span> os.path.isfile(filepath):<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;file_mtime = os.path.getmtime(filepath)<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;file_size = os.path.getsize(filepath)<br><br>
                        
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="code-keyword">if</span> file_mtime &lt; cutoff_time:<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="code-keyword">try</span>:<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;os.remove(filepath)<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;total_freed_space += file_size<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;deleted_files.append(filename)<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print(<span class="code-string">f"已删除: {filename} (大小: {file_size / 1024 / 1024:.2f}MB)"</span>)<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="code-keyword">except</span> Exception <span class="code-keyword">as</span> e:<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print(<span class="code-string">f"删除文件 {filename} 时出错: {e}"</span>)<br><br>
                        
                        &nbsp;&nbsp;&nbsp;&nbsp;<span class="code-comment"># 输出总结报告</span><br>
                        &nbsp;&nbsp;&nbsp;&nbsp;print(<span class="code-string">"\n===== 清理完成 ====="</span>)<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;print(<span class="code-string">f"共删除文件: {len(deleted_files)} 个"</span>)<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;print(<span class="code-string">f"共释放磁盘空间: {total_freed_space / 1024 / 1024:.2f} MB"</span>)<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;<span class="code-keyword">if</span> deleted_files:<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print(<span class="code-string">"被删除的文件列表:"</span>)<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="code-keyword">for</span> f <span class="code-keyword">in</span> deleted_files:<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print(<span class="code-string">f"  - {f}"</span>)<br><br>
                        
                        <span class="code-keyword">if</span> __name__ == <span class="code-string">"__main__"</span>:<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;parser = argparse.ArgumentParser(description=<span class="code-string">'清理旧日志文件'</span>)<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;parser.add_argument(<span class="code-string">'--dir'</span>, type=str, default=<span class="code-string">'/var/log/nginx/'</span>,<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;help=<span class="code-string">'要清理的目录路径 (默认: /var/log/nginx/)'</span>)<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;parser.add_argument(<span class="code-string">'--days'</span>, type=int, default=7,<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;help=<span class="code-string">'保留的天数 (默认: 7)'</span>)<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;args = parser.parse_args()<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;clean_old_logs(args.dir, args.days)<br>
                    </div>
                    
                    <h3>如何使用：</h3>
                    <p>1. <code>python3 clean_old_logs.py</code> （使用默认参数，清理/var/log/nginx/下7天前的日志）</p>
                    <p>2. <code>python3 clean_old_logs.py --dir /path/to/logs --days 30</code> （自定义目录和天数）</p>
                    
                    <h3>最终部署：</h3>
                    <p>使用 <code>crontab -e</code> 设置了一个定时任务，每周一凌晨2点自动运行这个脚本：</p>
                    <div class="code-block">
                        <span class="code-comment"># 每周一凌晨2点自动清理日志</span><br>
                        0 2 * * 1 /usr/bin/python3 /path/to/clean_old_logs.py > /dev/null 2>&1
                    </div>
                    
                    <div class="tip-box">
                        <p><strong>心得：</strong> 用一个几十行的Python脚本，就把重复性的手动工作自动化了，还避免了未来可能的磁盘危机。这让我真切地感受到了编程在运维工作中的威力。</p>
                    </div>
                </div>
            </article>
            
            <!-- 博客文章5 -->
            <article class="blog-post">
                <h2 class="post-title">"基础设施即代码"初探：我的Terraform学习笔记</h2>
                <div class="post-meta">
                    <span class="post-date">2024-06-15</span>
                    <div class="post-tags">
                        <span class="tag">Terraform</span>
                        <span class="tag">IaC</span>
                        <span class="tag">华为云</span>
                        <span class="tag">自动化</span>
                    </div>
                </div>
                
                <div class="post-content">
                    <p>手动在控制台点来点去创建资源，效率低且不容易复现。了解到"基础设施即代码"（IaC）的概念后，我决定学习目前最流行的Terraform来管理我的云资源。</p>
                    
                    <h3>我的理解：</h3>
                    <p>IaC就是把服务器、网络、存储等基础设施，用代码（配置文件）来定义和描述。好处是：</p>
                    <p>1. <strong>可重复</strong>：一键创建完全一样的环境。</p>
                    <p>2. <strong>可版本控制</strong>：用Git管理配置文件的变更，谁改了什么都一清二楚。</p>
                    <p>3. <strong>可协作</strong>：团队可以共同维护基础设施代码。</p>
                    
                    <h3>Terraform初体验：编写一个ECS部署脚本</h3>
                    <p>我的目标是写一个Terraform配置，让它能自动创建一台和我现在这台一模一样的ECS。</p>
                    
                    <p>1. <strong>安装与配置</strong>：从官网下载Terraform，并配置了华为云的Provider。</p>
                    <p>2. <strong>编写 main.tf</strong>：</p>
                    
                    <div class="code-block">
                        <span class="code-comment"># 指定华为云 Provider</span><br>
                        terraform {<br>
                        &nbsp;&nbsp;required_providers {<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;huaweicloud = {<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;source = <span class="code-string">"huaweicloud/huaweicloud"</span><br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;version = <span class="code-string">"~> 1.0"</span><br>
                        &nbsp;&nbsp;&nbsp;&nbsp;}<br>
                        &nbsp;&nbsp;}<br>
                        }<br><br>
                        
                        <span class="code-comment"># 配置提供商认证信息</span><br>
                        provider <span class="code-string">"huaweicloud"</span> {<br>
                        &nbsp;&nbsp;region = <span class="code-string">"cn-south-1"</span> <span class="code-comment"># 华南-广州</span><br>
                        }<br><br>
                        
                        <span class="code-comment"># 创建一台ECS实例</span><br>
                        resource <span class="code-string">"huaweicloud_compute_instance"</span> <span class="code-string">"my_web_server"</span> {<br>
                        &nbsp;&nbsp;name               = <span class="code-string">"my-web-server-from-tf"</span><br>
                        &nbsp;&nbsp;image_id           = <span class="code-string">"{{ 你的镜像ID }}"</span> <span class="code-comment"># 例如：CentOS 7.9</span><br>
                        &nbsp;&nbsp;flavor_id          = <span class="code-string">"s6.small.1"</span><br>
                        &nbsp;&nbsp;availability_zone  = <span class="code-string">"cn-south-1a"</span><br>
                        &nbsp;&nbsp;security_group_ids = [huaweicloud_networking_secgroup.my_secgroup.id]<br><br>
                        
                        &nbsp;&nbsp;network {<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;uuid = <span class="code-string">"{{ 你的VPC网络ID }}"</span><br>
                        &nbsp;&nbsp;}<br><br>
                        
                        &nbsp;&nbsp;system_disk_type = <span class="code-string">"SAS"</span><br>
                        &nbsp;&nbsp;system_disk_size = 40<br><br>
                        
                        &nbsp;&nbsp;<span class="code-comment"># 通过密码登录</span><br>
                        &nbsp;&nbsp;admin_pass = <span class="code-string">"YourStrongPassword123!"</span><br><br>
                        
                        &nbsp;&nbsp;<span class="code-comment"># 使用 cloud-init 脚本初始化，比如安装Nginx</span><br>
                        &nbsp;&nbsp;user_data = &lt;&lt;-EOF<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#!/bin/bash<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;yum install -y nginx<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;systemctl start nginx<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;systemctl enable nginx<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;EOF<br>
                        }<br><br>
                        
                        <span class="code-comment"># 创建一个安全组</span><br>
                        resource <span class="code-string">"huaweicloud_networking_secgroup"</span> <span class="code-string">"my_secgroup"</span> {<br>
                        &nbsp;&nbsp;name        = <span class="code-string">"my-tf-secgroup"</span><br>
                        &nbsp;&nbsp;description = <span class="code-string">"Security group created by Terraform"</span><br>
                        }<br><br>
                        
                        <span class="code-comment"># 配置安全组规则，开放80端口</span><br>
                        resource <span class="code-string">"huaweicloud_networking_secgroup_rule"</span> <span class="code-string">"http_ingress"</span> {<br>
                        &nbsp;&nbsp;security_group_id = huaweicloud_networking_secgroup.my_secgroup.id<br>
                        &nbsp;&nbsp;direction         = <span class="code-string">"ingress"</span><br>
                        &nbsp;&nbsp;ethertype         = <span class="code-string">"IPv4"</span><br>
                        &nbsp;&nbsp;protocol          = <span class="code-string">"tcp"</span><br>
                        &nbsp;&nbsp;port_range_min    = 80<br>
                        &nbsp;&nbsp;port_range_max    = 80<br>
                        &nbsp;&nbsp;remote_ip_prefix  = <span class="code-string">"0.0.0.0/0"</span><br>
                        }<br><br>
                        
                        <span class="code-comment"># 输出创建好的ECS的IP地址</span><br>
                        output <span class="code-string">"instance_ip"</span> {<br>
                        &nbsp;&nbsp;value = huaweicloud_compute_instance.my_web_server.access_ip_v4<br>
                        }<br>
                    </div>
                    
                    <h3>执行流程：</h3>
                    <p>1. <code>terraform init</code>：初始化，下载Provider。</p>
                    <p>2. <code>terraform plan</code>：生成执行计划，看看Terraform将要创建什么资源。<strong>（这步非常棒，能预览变更！）</strong></p>
                    <p>3. <code>terraform apply</code>：确认后，真正执行创建。等几分钟，一台安装了Nginx的ECS就创建好了！</p>
                    <p>4. <code>terraform destroy</code>：实验完后，一键销毁所有创建的资源，避免产生额外费用。</p>
                    
                    <div class="tip-box">
                        <p><strong>心得：</strong> 虽然第一次编写配置文件花了些时间，但想到以后能通过代码来快速复现一整套环境，就觉得这个学习成本太值了。Terraform把基础设施的管理变得像写程序一样优雅和可控。</p>
                    </div>
                </div>
            </article>
        </div>
        
        <footer>
            <p>© 2024 周越的技术博客 - 记录云计算学习之路</p>
            <p>联系方式: zy13005449599@icloud.com | GitHub: cqnhzyy</p>
        </footer>
    </div>
</body>
</html>
