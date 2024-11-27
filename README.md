const formatEmailBody = (emailBody) => {
    try {
        if (typeof emailBody !== "string") {
            throw new TypeError("emailBody must be a string");
        }
        return emailBody.split("\n").map(line => `<p>${line}</p>`).join("");
    } catch (error) {
        console.error("Error formatting email body:", error);
        return ""; // Fallback value
    }
};
