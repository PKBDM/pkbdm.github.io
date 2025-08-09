<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>单词学习软件</title>
    <!-- 引入Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- 引入Font Awesome图标 -->
    <link href="https://cdn.jsdelivr.net/npm/font-awesome@4.7.0/css/font-awesome.min.css" rel="stylesheet">
    
    <!-- 配置Tailwind自定义颜色 -->
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        primary: '#4F46E5',
                        secondary: '#10B981',
                    },
                    fontFamily: {
                        sans: ['Microsoft YaHei', 'sans-serif'],
                    },
                }
            }
        }
    </script>
    
    <!-- 自定义工具类 -->
    <style type="text/tailwindcss">
        @layer utilities {
            .content-auto {
                content-visibility: auto;
            }
            .text-shadow {
                text-shadow: 0 2px 4px rgba(0,0,0,0.1);
            }
            .transition-bounce {
                transition-timing-function: cubic-bezier(0.175, 0.885, 0.32, 1.275);
            }
        }
    </style>
</head>
<body class="bg-gradient-to-br from-indigo-50 to-emerald-50 min-h-screen flex items-center justify-center p-4">
    <div class="w-full max-w-md bg-white rounded-2xl shadow-xl overflow-hidden">
        <!-- 标题栏 -->
        <div class="bg-primary text-white p-4 text-center">
            <h1 class="text-xl font-bold">英语单词学习软件</h1>
        </div>
        
        <!-- 内容区域 -->
        <div class="p-6">
            <!-- 显示组合结果 -->
            <div class="flex items-center justify-center min-h-[180px] mb-8 border-2 border-gray-100 rounded-xl bg-gray-50">
                <p id="combination" class="text-[clamp(2rem,8vw,4rem)] font-bold text-gray-800 transition-all duration-300 text-shadow"></p>
            </div>
            
            <!-- 按钮 -->
            <button id="changeBtn" class="w-full bg-secondary hover:bg-secondary/90 text-white font-bold py-4 px-6 rounded-xl shadow-md transition-all duration-300 transform hover:scale-[1.02] active:scale-[0.98] flex items-center justify-center gap-2 text-lg">
                <i class="fa fa-refresh"></i>
                <span>更换组合</span>
            </button>
        </div>
    </div>

    <script>
        // 数据源 - 与Python代码相同
        const back = ["小乔","周瑜","大乔","蒙恬","李信","狂铁","李白","刘禅","刘备","关羽","张飞","曹操","女娲","妲己"];
        const front = ["旺仔","红牛","维他","怡宝","可乐","雪碧","脉动","尖叫"];
        
        // 获取DOM元素
        const combinationEl = document.getElementById('combination');
        const changeBtn = document.getElementById('changeBtn');
        
        // 初始显示第一个组合
        combinationEl.textContent = front[0] + back[0];
        
        // 打乱数组函数
        function shuffleArray(array) {
            // 创建数组副本，避免修改原数组
            const newArray = [...array];
            for (let i = newArray.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                // 交换元素
                [newArray[i], newArray[j]] = [newArray[j], newArray[i]];
            }
            return newArray;
        }
        
        // 更换组合的函数 - 实现与Python代码相同的逻辑
        function changeCombination() {
            // 保存当前的第一个元素
            const b0 = back[0];
            const f0 = front[0];
            
            // 打乱数组
            const shuffledBack = shuffleArray(back);
            const shuffledFront = shuffleArray(front);
            
            // 如果第一个元素与原来相同，则与第二个元素交换
            let newBack = [...shuffledBack];
            if (newBack[0] === b0) {
                [newBack[0], newBack[1]] = [newBack[1], newBack[0]];
            }
            
            let newFront = [...shuffledFront];
            if (newFront[0] === f0) {
                [newFront[0], newFront[1]] = [newFront[1], newFront[0]];
            }
            
            // 更新原始数组
            back.splice(0, back.length, ...newBack);
            front.splice(0, front.length, ...newFront);
            
            // 添加动画效果并更新显示
            combinationEl.classList.add('opacity-0', 'scale-95');
            setTimeout(() => {
                combinationEl.textContent = front[0] + back[0];
                combinationEl.classList.remove('opacity-0', 'scale-95');
            }, 200);
        }
        
        // 绑定按钮点击事件
        changeBtn.addEventListener('click', changeCombination);
    </script>
</body>
</html>
