// 更新时间函数
function updateTime() {
    const now = new Date();
    const timeElement = document.getElementById('time');
    timeElement.textContent = now.toLocaleString('zh-CN', {
        year: 'numeric',
        month: '2-digit',
        day: '2-digit',
        hour: '2-digit',
        minute: '2-digit',
        second: '2-digit'
    });
}

// 显示天气信息
function updateWeather() {
    const weatherElement = document.getElementById('weather');
    const weatherInfo = {
        location: '南京市',
        temperature: '2',
        condition: '局部多云',
        humidity: '70%',
        windDirection: '西北风',
        windLevel: '1级'
    };

    weatherElement.innerHTML = `
        <div>📍 ${weatherInfo.location}</div>
        <div>🌡️ ${weatherInfo.temperature}°C</div>
        <div>☁️ ${weatherInfo.condition}</div>
        <div>💨 ${weatherInfo.windDirection} ${weatherInfo.windLevel}</div>
        <div>💧 湿度: ${weatherInfo.humidity}</div>
    `;
}

// 每秒更新时间
setInterval(updateTime, 1000);
updateTime();

// 显示天气信息
updateWeather(); 

// 存储编辑历史
const editHistory = {
    'boy-content': [],
    'girl-content': []
};

// 添加文字
function addText(contentId) {
    const text = prompt('请输入文字内容：');
    if (text) {
        const contentDiv = document.getElementById(contentId);
        const item = document.createElement('div');
        item.className = 'content-item';
        item.innerHTML = `
            ${text}
            <button class="delete-btn" onclick="deleteItem(this)">×</button>
        `;
        contentDiv.appendChild(item);
        saveToHistory(contentId, item.cloneNode(true));
    }
}

// 添加图片
function addImage(contentId) {
    const imageUrl = prompt('请输入图片URL：');
    if (imageUrl) {
        const contentDiv = document.getElementById(contentId);
        const item = document.createElement('div');
        item.className = 'content-item';
        item.innerHTML = `
            <img src="${imageUrl}" alt="个人照片">
            <button class="delete-btn" onclick="deleteItem(this)">×</button>
        `;
        contentDiv.appendChild(item);
        saveToHistory(contentId, item.cloneNode(true));
    }
}

// 删除内容
function deleteItem(button) {
    const item = button.parentElement;
    const contentId = item.parentElement.id;
    saveToHistory(contentId, item.cloneNode(true));
    item.remove();
}

// 保存到历史记录
function saveToHistory(contentId, item) {
    editHistory[contentId].push({
        content: item,
        timestamp: new Date().getTime()
    });
}

// 撤销操作
function undo(contentId) {
    if (editHistory[contentId].length > 0) {
        const contentDiv = document.getElementById(contentId);
        const lastState = editHistory[contentId].pop();
        contentDiv.innerHTML = Array.from(contentDiv.children)
            .filter(child => child.dataset.timestamp !== lastState.timestamp)
            .map(child => child.outerHTML)
            .join('');
    }
} 

// 在现有代码后添加时间轴功能

// 存储时间轴事件
let timelineEvents = [];

// 修改保存事件的函数
function saveTimelineEvent() {
    const date = document.getElementById('event-date').value;
    const title = document.getElementById('event-title').value;
    const description = document.getElementById('event-description').value;
    const imageElements = document.getElementById('event-images').getElementsByTagName('img');
    const images = Array.from(imageElements).map(img => img.src);

    if (!date || !title) {
        alert('请至少填写日期和标题！');
        return;
    }

    const event = {
        date,
        title,
        description,
        images,
        id: Date.now(),
        side: timelineEvents.length % 2 === 0 ? 'left' : 'right'
    };

    timelineEvents.push(event);
    renderTimelineEvents();

    // 清空表单
    document.getElementById('event-date').value = '';
    document.getElementById('event-title').value = '';
    document.getElementById('event-description').value = '';
    document.getElementById('event-images').innerHTML = '';
}

// 修改渲染时间轴事件的函数
function renderTimelineEvents() {
    const eventsContainer = document.getElementById('timeline-events');
    const photosContainer = document.getElementById('timeline-photos');
    
    eventsContainer.innerHTML = '';
    photosContainer.innerHTML = '';
    
    timelineEvents.sort((a, b) => new Date(b.date) - new Date(a.date));
    
    timelineEvents.forEach(event => {
        // 渲染事件
        const eventElement = document.createElement('div');
        eventElement.className = 'timeline-event';
        eventElement.innerHTML = `
            <div class="timeline-date">${formatDate(event.date)}</div>
            <h3>${event.title}</h3>
            ${event.description ? `<p>${event.description}</p>` : ''}
            <button class="delete-btn" onclick="deleteTimelineEvent(${event.id})">×</button>
            <button class="edit-btn" onclick="editTimelineEvent(${event.id})">✎</button>
        `;
        eventsContainer.appendChild(eventElement);

        // 渲染照片
        if (event.images && event.images.length > 0) {
            event.images.forEach(imageUrl => {
                const photoElement = document.createElement('div');
                photoElement.className = 'timeline-photo';
                photoElement.innerHTML = `
                    <img src="${imageUrl}" alt="回忆照片">
                    <div class="photo-date">${formatDate(event.date)}</div>
                `;
                photosContainer.appendChild(photoElement);
            });
        }
    });
}

// 格式化日期
function formatDate(dateString) {
    const date = new Date(dateString);
    return date.toLocaleDateString('zh-CN', {
        year: 'numeric',
        month: 'long',
        day: 'numeric'
    });
}

// 编辑时间轴事件
function editTimelineEvent(id) {
    const event = timelineEvents.find(e => e.id === id);
    if (!event) return;

    document.getElementById('event-date').value = event.date;
    document.getElementById('event-title').value = event.title;
    document.getElementById('event-description').value = event.description || '';

    const imagesContainer = document.getElementById('event-images');
    imagesContainer.innerHTML = '';
    if (event.images) {
        event.images.forEach(imageUrl => {
            const imageContainer = document.createElement('div');
            imageContainer.className = 'event-image-container';
            imageContainer.innerHTML = `
                <img src="${imageUrl}" alt="事件图片">
                <button class="delete-btn" onclick="this.parentElement.remove()">×</button>
            `;
            imagesContainer.appendChild(imageContainer);
        });
    }

    document.getElementById('event-modal').style.display = 'block';
    
    // 保存时删除旧事件
    const oldSaveEvent = window.saveTimelineEvent;
    window.saveTimelineEvent = function() {
        timelineEvents = timelineEvents.filter(e => e.id !== id);
        oldSaveEvent();
        window.saveTimelineEvent = oldSaveEvent;
    };
}

// 关闭模态框的点击事件
document.querySelector('.close-modal').onclick = closeEventModal;
window.onclick = function(event) {
    const modal = document.getElementById('event-modal');
    if (event.target === modal) {
        closeEventModal();
    }
};

// 删除时间轴事件
function deleteTimelineEvent(id) {
    if (confirm('确定要删除这个事件吗？')) {
        timelineEvents = timelineEvents.filter(event => event.id !== id);
        renderTimelineEvents();
    }
}

// 初始化时间轴
renderTimelineEvents(); 

// 修改图片压缩功能
function compressImage(file, maxSize = 800, quality = 0.8) {
    return new Promise((resolve) => {
        const reader = new FileReader();
        reader.readAsDataURL(file);
        reader.onload = function(e) {
            const img = new Image();
            img.src = e.target.result;
            
            img.onload = function() {
                let width = img.width;
                let height = img.height;
                
                // 计算等比例缩放
                const maxDimension = Math.max(width, height);
                if (maxDimension > maxSize) {
                    const ratio = maxSize / maxDimension;
                    width = Math.round(width * ratio);
                    height = Math.round(height * ratio);
                }
                
                // 创建 canvas 进行压缩
                const canvas = document.createElement('canvas');
                canvas.width = width;
                canvas.height = height;
                const ctx = canvas.getContext('2d');
                
                // 使用更好的图像平滑算法
                ctx.imageSmoothingEnabled = true;
                ctx.imageSmoothingQuality = 'high';
                
                // 绘制图片
                ctx.drawImage(img, 0, 0, width, height);
                
                // 转换为 base64，使用更高的质量设置
                const compressedDataUrl = canvas.toDataURL('image/jpeg', quality);
                
                // 添加图片尺寸信息到返回数据
                resolve({
                    dataUrl: compressedDataUrl,
                    width,
                    height
                });
            };
        };
    });
}

// 修改个人信息区域的图片上传函数
async function handleImageUpload(input, contentId) {
    if (input.files && input.files[0]) {
        const file = input.files[0];
        try {
            const contentDiv = document.getElementById(contentId);
            const loadingItem = document.createElement('div');
            loadingItem.className = 'content-item loading';
            loadingItem.textContent = '图片处理中...';
            contentDiv.appendChild(loadingItem);

            const compressed = await compressImage(file);
            loadingItem.remove();
            
            const item = document.createElement('div');
            item.className = 'content-item';
            item.innerHTML = `
                <img src="${compressed.dataUrl}" alt="个人照片" style="width: 100%; max-width: ${compressed.width}px;">
                <button class="delete-btn" onclick="deleteItem(this)">×</button>
                <div class="image-info">已优化 ✨ ${compressed.width}×${compressed.height}</div>
            `;
            contentDiv.appendChild(item);
            saveToHistory(contentId, item.cloneNode(true));
        } catch (error) {
            console.error('图片处理失败:', error);
            alert('图片处理失败，请重试');
        }
        
        input.value = '';
    }
}

// 修改时间轴事件的图片上传函数
async function handleEventImageUpload(input) {
    const imagesContainer = document.getElementById('event-images');
    
    if (input.files) {
        const files = Array.from(input.files);
        const loadingDiv = document.createElement('div');
        loadingDiv.className = 'loading-message';
        loadingDiv.textContent = '正在处理图片...';
        imagesContainer.appendChild(loadingDiv);
        
        try {
            const processedImages = await Promise.all(files.map(async (file) => {
                return await compressImage(file);
            }));
            
            loadingDiv.remove();
            
            processedImages.forEach(compressed => {
                const imageContainer = document.createElement('div');
                imageContainer.className = 'image-preview';
                imageContainer.innerHTML = `
                    <img src="${compressed.dataUrl}" alt="事件图片" style="width: 100%; max-width: ${compressed.width}px;">
                    <button class="delete-btn" onclick="this.parentElement.remove()">×</button>
                    <div class="image-info">已优化 ✨ ${compressed.width}×${compressed.height}</div>
                `;
                imagesContainer.appendChild(imageContainer);
            });
        } catch (error) {
            console.error('图片处理失败:', error);
            alert('部分图片处理失败，请重试');
            loadingDiv.remove();
        }
    }
}
 