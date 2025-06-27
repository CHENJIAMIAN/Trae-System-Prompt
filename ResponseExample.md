## Example1
{
    "toolcall": {
        "thought": "用户想让我写一个浏览器插件，这是一个比较宽泛的需求，我需要先了解用户希望插件实现什么功能，以及目标浏览器是什么。我将使用 `finish` 工具来回应用户，并询问这些关键信息。",
        "name": "finish",
        "params": {
            "summary": "你好！我很乐意帮你写一个浏览器插件。为了更好地满足你的需求，请你提供更多信息：\n\n1.  **插件的功能：** 你希望这个插件实现什么特定的功能？例如，是网页内容抓取、广告拦截、密码管理、网页样式修改，还是其他功能？\n2.  **目标浏览器：** 你希望这个插件支持哪个浏览器？（例如：Chrome, Firefox, Edge 等）\n3.  **其他需求：** 有没有其他你想要实现的具体细节或者设计要求？\n\n请告诉我你的想法，我将根据你的需求来协助你。"
        },
        "query_language": "Chinese"
    }
}

## Example2
{
    "toolcall": {
        "thought": "好的，我将开始创建浏览器插件的文件。首先，我将创建 `manifest.json` 文件来定义插件的基本信息和结构。",
        "name": "write_to_file",
        "params": {
            "rewrite": false,
            "file_path": "d:\\Desktop\\2025年6月26日\\browser-ai-summary-extension\\manifest.json",
            "content": "{\n  \"manifest_version\": 3,\n  \"name\": \"AI Page Summarizer\",\n  \"version\": \"1.0\",\n  \"description\": \"Summarizes the current page content using AI.\",\n  \"permissions\": [\n    \"activeTab\",\n    \"scripting\",\n    \"storage\"\n  ],\n  \"host_permissions\": [\n    \"<all_urls>\"\n  ],\n  \"action\": {\n    \"default_popup\": \"popup.html\",\n    \"default_icon\": {\n      \"16\": \"icons/icon16.png\",\n      \"48\": \"icons/icon48.png\",\n      \"128\": \"icons/icon128.png\"\n    }\n  },\n  \"background\": {\n    \"service_worker\": \"background.js\"\n  },\n  \"content_scripts\": [\n    {\n      \"matches\": [\"<all_urls>\"],\n      \"js\": [\"content.js\"]\n    }\n  ],\n  \"web_accessible_resources\": [\n    {\n      \"resources\": [ \"icons/*.png\" ],\n      \"matches\": [ \"<all_urls>\" ]\n    }\n  ]\n}"
        },
        "query_language": "Chinese"
    }
}