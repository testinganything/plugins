import { findByProps } from "@vendetta/metro";
import { instead } from "@vendetta/patcher";

const messageInputUtil = findByProps("selectGIF") || findByProps("selectGif"); // Fallback for case sensitivity or rename

export const onUnload = () => {}; // Default unload in case patch fails

if (messageInputUtil) {
    export const onUnload = instead("selectGIF", messageInputUtil, (args, original) => {
        try {
            // Handle old ([GIF]) or new (channelId, [GIF]) signatures
            const gifData = args[1] || args[0];
            if (!gifData || !gifData.url) throw new Error("No GIF URL found");

            const url = gifData.url;
            if (args[1]) args[1] = url; // Newer version
            else args[0] = url;        // Older version

            // Check if insertText exists, fallback to original if not
            if (messageInputUtil.insertText) {
                messageInputUtil.insertText(...args);
            } else {
                console.error("insertText not found, falling back to original");
                original(...args);
            }
        } catch (e) {
            console.error("GIF plugin error:", e);
            original(...args); // Don’t break everything if it fails
        }
    });
} else {
    console.error("Couldn’t find selectGIF—Discord might’ve renamed it.");
}
