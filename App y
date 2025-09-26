import streamlit as st
import chess
import chess.engine
from streamlit_chessboard import st_chessboard

# Page setup
st.set_page_config(page_title="Chess.com Rahul", page_icon="♟️", layout="centered")
st.title("♟️ Chess.com Rahul - Drag & Drop Edition")

# Initialize game state
if "board" not in st.session_state:
    st.session_state.board = chess.Board()
if "history" not in st.session_state:
    st.session_state.history = []

# Show draggable chessboard
board_state = st_chessboard(
    fen=st.session_state.board.fen(),
    key="chessboard"
)

# If user makes a move
if board_state and board_state != st.session_state.board.fen():
    try:
        diff_board = chess.Board(board_state)
        moves_made = len(diff_board.move_stack) - len(st.session_state.board.move_stack)
        if moves_made == 1:
            move = diff_board.move_stack[-1]
            st.session_state.board.push(move)
            st.session_state.history.append(f"Player: {move.uci()}")

            # Stockfish reply
            try:
                engine = chess.engine.SimpleEngine.popen_uci("/usr/bin/stockfish")
                result = engine.play(st.session_state.board, chess.engine.Limit(time=0.1))
                st.session_state.board.push(result.move)
                st.session_state.history.append(f"Stockfish: {result.move.uci()}")
                engine.quit()
            except Exception as e:
                st.error(f"Engine error: {e}")
    except Exception as e:
        st.warning("Something went wrong with the move.")

# Move history
st.subheader("Move History")
for h in st.session_state.history:
    st.write(h)

# Game result
if st.session_state.board.is_game_over():
    st.success(f"Game Over: {st.session_state.board.result()}")
