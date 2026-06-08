document.addEventListener("DOMContentLoaded", () => {
  const scoreInputs = document.querySelectorAll(".domain-score");

  const totalDomainScoreInput = document.getElementById("total-domain-score");
  const questionsAnsweredInput = document.getElementById("questions-answered");
  const controlScoreInput = document.getElementById("control-score");

  const nonNaCountInput = document.getElementById("non-na-count");
  const engagementScoreInput = document.getElementById("engagement-score");

  const totalQuestionsInDomain = scoreInputs.length;

  function isNA(value) {
    const cleaned = value.trim().toLowerCase();

    return (
      cleaned === "n.a." ||
      cleaned === "na" ||
      cleaned === "n/a" ||
      cleaned === "not applicable"
    );
  }

  function calculateScores() {
    let totalDomainScore = 0;
    let questionsAnswered = 0;

    scoreInputs.forEach((input) => {
      const value = input.value.trim();

      if (value === "" || isNA(value)) {
        return;
      }

      const score = Number(value);

      if (!Number.isNaN(score) && score >= 0 && score <= 4) {
        totalDomainScore += score;
        questionsAnswered += 1;
      }
    });

    const controlScore =
      questionsAnswered > 0
        ? totalDomainScore / (questionsAnswered * 4)
        : "";

    const engagementScore =
      totalQuestionsInDomain > 0
        ? questionsAnswered / totalQuestionsInDomain
        : "";

    totalDomainScoreInput.value = totalDomainScore;
    questionsAnsweredInput.value = questionsAnswered;

    controlScoreInput.value =
      controlScore === "" ? "" : controlScore.toFixed(2);

    nonNaCountInput.value = questionsAnswered;

    engagementScoreInput.value =
      engagementScore === "" ? "" : engagementScore.toFixed(2);
  }

  scoreInputs.forEach((input) => {
    input.addEventListener("input", calculateScores);
  });

  calculateScores();
});